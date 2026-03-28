# Skill: Firebase Ops

## Purpose
Read, write, and manage Firebase (Auth, Realtime DB, Firestore, Cloud Functions, Storage) for mobile apps.

## Activation
Activate when the task involves:
- Firebase Authentication (sign in, sign up, tokens)
- Realtime Database or Firestore reads/writes
- Cloud Functions (triggers, callable functions)
- Firebase Storage
- Security rules

## Data Modeling Rules

### Realtime Database
```
// Denormalize for read speed — never nest more than 3 levels
{
  "users": {
    "$uid": {
      "profile": { "name": "...", "joinedAt": "..." },
      "stats": { "territories": 0, "totalDistance": 0 }
    }
  },
  "territories": {
    "$territoryId": {
      "ownerId": "...",
      "claimedAt": "...",
      "geometry": { ... }
    }
  },
  // Fan-out pattern for user → territory mapping
  "userTerritories": {
    "$uid": {
      "$territoryId": true
    }
  }
}
```

### Firestore
- One collection per entity type
- Subcollections only when the child is never queried without the parent
- Always include `createdAt` and `updatedAt` (Timestamp) on every document

## Service Pattern
```typescript
// services/territories.ts — all Firebase logic here, no UI imports
import { ref, set, onValue } from 'firebase/database';
import { db } from '../config/firebase';

export async function claimTerritory(uid: string, territory: Territory): Promise<void> {
  const updates: Record<string, unknown> = {
    [`territories/${territory.id}`]: { ...territory, ownerId: uid },
    [`userTerritories/${uid}/${territory.id}`]: true,
  };
  await update(ref(db), updates); // Atomic multi-path write
}
```

## Security Rules Checklist
Before any new data write, verify:
- [ ] Auth check: `request.auth != null`
- [ ] Ownership check: `request.auth.uid == resource.data.userId`
- [ ] Input validation: type and range checks on written values
- [ ] Rate limit consideration: note if function needs throttling

## Cloud Functions Template
```typescript
// functions/src/index.ts
import { onCall, HttpsError } from 'firebase-functions/v2/https';

export const myFunction = onCall({ region: 'us-central1' }, async (request) => {
  if (!request.auth) throw new HttpsError('unauthenticated', 'Login required');
  // logic
});
```

## Output
- Service files in `src/services/`
- Updated security rules in `firestore.rules` or `database.rules.json`
- Cloud Function files in `functions/src/`
