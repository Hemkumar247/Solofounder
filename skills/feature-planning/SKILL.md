# Skill: Feature Planning

## Purpose
Turn a vague feature request into a concrete, implementable design note.

## Activation
Activate when the task starts with design or planning — before any code is written.

## Process

### Step 1: Clarify (1-2 focused questions max)
If the request is ambiguous, ask only the questions that would change the design.

### Step 2: Produce a Design Note

```markdown
# Design Note: [Feature Name]
**Date:** [ISO date]
**Requested by:** Human
**Status:** Draft | Approved

## Problem
[One paragraph: what user problem does this solve?]

## Proposed Approach
[How we'll solve it — data model, component structure, API contracts]

## Data Model Changes
[Schema diff — new fields, new collections, index requirements]

## Component Impact
[Which existing components are touched, what new ones are created]

## Dependencies
[External APIs, new packages, Firebase rules changes needed]

## Tradeoffs
| Option | Pros | Cons |
|--------|------|------|
| Chosen approach | ... | ... |
| Alternative considered | ... | ... |

## Risks
- 🚧 [Blocking risk, if any]
- ⚠️ [Non-blocking risk]

## Acceptance Criteria
- [ ] AC1: [specific, testable]
- [ ] AC2: ...
```

### Step 3: Save and Delegate
- Save design note to `memory/runtime/key-decisions.md`
- Pass to Coder agent with explicit reference: "See design note: [Feature Name]"

## Output
- Design note in `memory/runtime/key-decisions.md`
- Updated `knowledge/codebase-map.md` if new patterns are introduced
