# Skill: React Native Dev

## Purpose
Build, modify, and debug React Native components and screens for Expo-based mobile apps.

## Activation
Activate when the task involves:
- Creating or editing React Native components
- Working with navigation (React Navigation v6+)
- State management (Zustand, Jotai, or Context)
- Animations (React Native Reanimated)
- Mobile-specific APIs (SafeAreaView, Keyboard, Haptics, StatusBar)

## Conventions

### File Structure
```
src/
  components/       # Reusable UI components
  screens/          # Screen-level components (one per route)
  hooks/            # Custom React hooks
  store/            # Global state (Zustand slices)
  services/         # Firebase, API calls (no UI logic)
  utils/            # Pure functions
  types/            # Shared TypeScript types
  theme/            # Colors, typography, spacing tokens
  navigation/       # Navigator definitions
```

### Component Template
```tsx
import React from 'react';
import { View, StyleSheet } from 'react-native';
import { Colors, Spacing } from '../theme';

interface Props {
  // Explicit prop types always
}

/**
 * [Component purpose in one sentence]
 */
export const MyComponent: React.FC<Props> = ({ }) => {
  return (
    <View style={styles.container}>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    // Use theme tokens, never hardcoded colors
    backgroundColor: Colors.background,
    padding: Spacing.md,
  },
});
```

### Key Rules
- Always use `StyleSheet.create()` — never inline objects in render
- Always use theme tokens for colors and spacing
- Wrap screen root in `<SafeAreaView>` with edges specified
- Use `useCallback` and `useMemo` for handlers passed to lists
- Never fetch data directly in a component — use a service + hook pattern

### Animation (Reanimated)
```tsx
// Spring physics always — never linear for UI elements
const animatedStyle = useAnimatedStyle(() => ({
  transform: [{ scale: withSpring(scale.value, { damping: 15, stiffness: 120 }) }],
}));
```

## Output
- `.tsx` files in `src/components/` or `src/screens/`
- Matching `*.test.tsx` file passed to QA agent
- Update `knowledge/codebase-map.md` if a new pattern is introduced
