# Codebase Map

> Maintained by the Architect agent. Updated whenever a new pattern or directory is added.

## Project Root Structure

```
my-project/
├── src/
│   ├── components/          # Reusable UI (never fetch data here)
│   ├── screens/             # One file per navigation route
│   ├── hooks/               # Custom React hooks (useTerritory, useAuth, etc.)
│   ├── services/            # All Firebase calls live here
│   ├── store/               # Zustand slices
│   ├── utils/               # Pure utility functions
│   ├── types/               # Shared TypeScript interfaces & types
│   ├── theme/               # Colors, typography, spacing
│   └── navigation/          # Navigator definitions
├── functions/               # Firebase Cloud Functions
│   └── src/
├── assets/                  # Images, fonts, icons
├── __tests__/               # Top-level integration tests
├── .env.example             # Required env vars (no values)
├── app.json                 # Expo config
├── firebase.json            # Firebase project config
├── firestore.rules          # Firestore security rules (if using Firestore)
├── database.rules.json      # Realtime DB security rules
└── agent.yaml               # This SoloFounder agent manifest
```

## Key Patterns Catalogue

| Pattern | File Example | Purpose |
|---------|-------------|---------|
| Service + Hook | `services/territories.ts` + `hooks/useTerritory.ts` | Data fetching decoupled from UI |
| Zustand slice | `store/userSlice.ts` | Global state per domain |
| Theme token | `theme/colors.ts` | Never hardcode colors |
| Reanimated spring | `components/TerritoryCard.tsx` | All UI animations |
| Fan-out write | `services/territories.ts#claimTerritory` | Atomic multi-path Firebase updates |

## Anti-Patterns (never do these)

| Anti-pattern | Why |
|-------------|-----|
| `firebase.database().ref()` in a component | Breaks service abstraction |
| Inline styles: `style={{ color: '#fff' }}` | Hardcoded, not theme-able |
| `.map()` over 20+ items in render | Use FlatList |
| `any` in TypeScript | Defeats type safety |
| Direct state mutation | Use Zustand setters |
