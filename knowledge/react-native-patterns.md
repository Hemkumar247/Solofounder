# React Native Patterns

> Maintained by the Architect agent. Updated whenever a new approved pattern or anti-pattern is confirmed in code review.
> Stack: React Native + Expo, TypeScript, Zustand, Firebase Realtime DB, React Native Reanimated, React Navigation v6

---

## ✅ Approved Patterns

### 1. Service + Hook Pattern (data fetching)

All Firebase reads/writes must live in `src/services/`, never in components or screens directly.
A custom hook in `src/hooks/` wraps the service and exposes reactive state to the UI.

```typescript
// src/services/territories.ts
import { database } from '../firebase';
import { ref, onValue, off } from 'firebase/database';

export function subscribeToUserTerritories(
  uid: string,
  callback: (territoryIds: string[]) => void
): () => void {
  const dbRef = ref(database, `userTerritories/${uid}`);
  onValue(dbRef, (snapshot) => {
    const data = snapshot.val() ?? {};
    callback(Object.keys(data)); // keys are territory ID strings
  });
  return () => off(dbRef); // always return cleanup
}

// src/hooks/useUserTerritories.ts
import { useEffect, useState } from 'react';
import { subscribeToUserTerritories } from '../services/territories';

export function useUserTerritories(uid: string) {
  const [territoryIds, setTerritoryIds] = useState<string[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const unsubscribe = subscribeToUserTerritories(uid, (ids) => {
      setTerritoryIds(ids);
      setLoading(false);
    });
    return unsubscribe; // cleanup on unmount — no listener leak
  }, [uid]);

  return { territoryIds, loading };
}
```

**Why:** Keeps components pure, makes services independently testable, prevents Firebase listener leaks.

---

### 2. Zustand Slice Pattern (global state)

One file per domain slice. Selectors should be granular to avoid unnecessary re-renders.

```typescript
// src/store/userSlice.ts
import { create } from 'zustand';

interface UserState {
  uid: string | null;
  displayName: string | null;
  setUser: (uid: string, displayName: string) => void;
  clearUser: () => void;
}

export const useUserStore = create<UserState>((set) => ({
  uid: null,
  displayName: null,
  setUser: (uid, displayName) => set({ uid, displayName }),
  clearUser: () => set({ uid: null, displayName: null }),
}));
```

**Why:** Predictable, devtools-friendly, avoids prop drilling.

---

### 3. Theme Token Usage

Always import from `../theme` — never hardcode colors, spacing, or border radii.

```typescript
import { Colors, Spacing, Radius, Typography } from '../theme';

const styles = StyleSheet.create({
  card: {
    backgroundColor: Colors.glass2,
    borderWidth: 1,
    borderColor: Colors.borderSubtle,
    borderRadius: Radius.lg,
    padding: Spacing.md,
  },
  title: {
    ...Typography.displayMedium,
    color: Colors.textPrimary,
  },
});
```

---

### 4. Reanimated Spring Animations

Use `withSpring` for all interactive UI animations. Never use `Animated` from React Native core.

```typescript
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
} from 'react-native-reanimated';

const scale = useSharedValue(1);

const animatedStyle = useAnimatedStyle(() => ({
  transform: [{ scale: withSpring(scale.value, { damping: 15, stiffness: 120 }) }],
}));

// On press
scale.value = 0.95; // triggers spring back automatically
```

Standard spring presets (from design-tokens.md):
| Use case | damping | stiffness |
|----------|---------|-----------|
| Standard interaction | 15 | 120 |
| Snappy button press | 20 | 200 |
| Slow entrance | 25 | 80 |

---

### 5. FlatList for Long Lists

Use `FlatList` (or `FlashList` from `@shopify/flash-list` for > 100 items) for any list that can
exceed 20 items. Never use `.map()` in JSX for variable-length data.

```typescript
import { FlatList } from 'react-native';

<FlatList
  data={territories}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => <TerritoryCard territory={item} />}
  contentContainerStyle={{ padding: Spacing.md }}
/>
```

---

### 6. Fan-out Write for Multi-path Firebase Updates

When a single user action must update multiple nodes atomically, use a fan-out write (multi-path update).

```typescript
// src/services/territories.ts
import { ref, update } from 'firebase/database';
import { database } from '../firebase';

export async function claimTerritory(uid: string, territoryId: string, data: Territory) {
  const updates: Record<string, unknown> = {
    [`territories/${territoryId}`]: data,
    [`userTerritories/${uid}/${territoryId}`]: true,
    [`users/${uid}/stats/totalTerritories`]: data.totalCount,
  };
  await update(ref(database), updates); // single atomic write
}
```

---

### 7. Screen Component Structure

Every screen wraps its root in `<SafeAreaView>` with explicit edges. No data fetching inline.

```typescript
import { SafeAreaView } from 'react-native-safe-area-context';

export const MapScreen: React.FC = () => {
  const { uid } = useUserStore((s) => ({ uid: s.uid }));
  const { territories, loading } = useUserTerritories(uid!);

  return (
    <SafeAreaView edges={['top', 'bottom']} style={styles.container}>
      {/* screen content */}
    </SafeAreaView>
  );
};
```

---

## ❌ Anti-Patterns

| Anti-pattern | Why it's banned | Correct alternative |
|---|---|---|
| `firebase.database().ref()` inside a component | Breaks service abstraction, cannot be unit-tested | Move to `src/services/` |
| `onValue(ref, cb)` without cleanup in `useEffect` | Firebase listener leak — memory leak and stale callbacks | Always `return () => off(dbRef)` |
| Inline style objects: `style={{ color: '#fff' }}` | Creates new object every render, bypasses theme tokens | Use `StyleSheet.create` + theme tokens |
| `.map()` over dynamic lists in JSX | Performance degrades beyond ~20 items, no recycling | Use `FlatList` or `FlashList` |
| `any` in TypeScript | Defeats type safety, hides bugs at compile time | Use explicit interfaces from `src/types/` |
| Direct Zustand state mutation | Zustand state is immutable by convention | Use store setter functions |
| `console.log` in production paths | Leaks PII, bloats logs | Remove before PR; use proper error reporting |
| Hardcoded color/spacing values | Non-themeable, diverges from design system | Import from `src/theme/` |
| `useEffect` with missing deps | Stale closures, hard-to-debug re-render bugs | Satisfy ESLint `exhaustive-deps` rule |
| Navigation params for large data objects | Serialization overhead, param size limits | Pass IDs and fetch in destination screen |

---

## Glass Component Recipe (quick reference)

```typescript
// Standard glass card
const styles = StyleSheet.create({
  card: {
    backgroundColor: Colors.glass2,
    borderWidth: 1,
    borderColor: Colors.borderSubtle,
    borderRadius: Radius.lg,
  },
  cardActive: {
    backgroundColor: Colors.glass3,
    borderColor: Colors.borderStrong, // cyan glow when active
  },
});

// Pair with BlurView for true glass effect (iOS)
// import { BlurView } from '@react-native-community/blur';
// <BlurView blurType="dark" blurAmount={20} style={StyleSheet.absoluteFill} />
```
