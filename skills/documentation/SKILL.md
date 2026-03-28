# Skill: Documentation

## Purpose
Generate and maintain README, API docs, and inline code documentation.

## Activation
Activate when:
- A new feature is merged (update README)
- A new service or hook is created (generate JSDoc)
- The human asks for documentation

## README Template
```markdown
# [Project Name]

[One-sentence tagline]

## What it does
[2-3 sentences. Focus on the user value, not the tech stack.]

## Quick Start
\`\`\`bash
git clone [repo]
cd [project]
npm install
cp .env.example .env   # Fill in your keys
npx expo start
\`\`\`

## Architecture
[Link to knowledge/codebase-map.md or a brief summary]

## Agent Team
This project is built with [SoloFounder](https://github.com/your-handle/solofoundер).
See `agent.yaml` for the full team configuration.

## Contributing
[PR process, branch naming, how to run tests]
```

## JSDoc Standard
```typescript
/**
 * Claims a geographic territory for the given user.
 * 
 * @param uid - Firebase Auth user ID of the claiming user
 * @param territory - Territory object with geometry and metadata
 * @throws {FirebaseError} If the write fails or is rejected by security rules
 * @returns Promise that resolves when the claim is committed
 */
export async function claimTerritory(uid: string, territory: Territory): Promise<void>
```

## Output
- Updated `README.md` in project root
- JSDoc comments added inline to exported functions
