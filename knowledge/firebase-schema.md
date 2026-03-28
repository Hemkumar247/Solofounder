# Firebase Data Schema

> Maintained by Architect agent. Updated on every schema change.
> Stack: Firebase Realtime Database (primary) + Firebase Auth

## Auth

Provider: Firebase Auth
Supported methods: Email/password, Google OAuth
User ID: `firebase.auth().currentUser.uid` — used as primary key everywhere

## Realtime Database Schema

```json
{
  "users": {
    "$uid": {
      "profile": {
        "displayName": "string",
        "photoURL": "string | null",
        "joinedAt": "number (Unix ms)",
        "lastActiveAt": "number (Unix ms)"
      },
      "stats": {
        "totalTerritories": "number",
        "totalDistanceMeters": "number",
        "totalRunningSessionsMs": "number",
        "level": "number"
      }
    }
  },

  "territories": {
    "$territoryId": {
      "ownerId": "string (uid)",
      "claimedAt": "number (Unix ms)",
      "lastDefendedAt": "number (Unix ms)",
      "geometry": {
        "type": "Polygon",
        "coordinates": "[[lng, lat], ...]"
      },
      "centroid": {
        "lat": "number",
        "lng": "number"
      },
      "area": "number (square meters)",
      "name": "string | null"
    }
  },

  "userTerritories": {
    "$uid": {
      "$territoryId": true
    }
  },

  "leaderboard": {
    "byTerritories": {
      "$uid": "number (territory count)"
    },
    "byDistance": {
      "$uid": "number (total distance)"
    }
  },

  "sessions": {
    "$uid": {
      "$sessionId": {
        "startedAt": "number",
        "endedAt": "number | null",
        "distanceMeters": "number",
        "territoriesClaimed": ["string (territoryId)"]
      }
    }
  }
}
```

## Security Rules Summary

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    },
    "territories": {
      ".read": "auth !== null",
      "$territoryId": {
        ".write": "auth !== null"
      }
    },
    "userTerritories": {
      "$uid": {
        ".read": "auth !== null",
        ".write": "$uid === auth.uid"
      }
    },
    "leaderboard": {
      ".read": "auth !== null",
      ".write": false
    }
  }
}
```

Note: Leaderboard updates are handled by Cloud Functions only (never direct client writes).
