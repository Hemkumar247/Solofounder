# Agents — SoloFounder

Framework-agnostic fallback instructions. When no specific adapter is used,
these instructions govern how the agent system operates.

---

## Team Overview

SoloFounder is a four-agent team. Each agent has a dedicated subdirectory
under `agents/` with its own `agent.yaml` and `SOUL.md`. The root agent
coordinates delegation.

| Agent | Role | Triggers |
|-------|------|----------|
| `architect` | Designs systems and makes technical decisions | "design", "architect", "plan", "how should we structure" |
| `coder` | Implements features and fixes bugs | "implement", "build", "fix", "code", "write the" |
| `reviewer` | Reviews code and opens PR feedback | "review", "check this", "is this correct" |
| `qa-engineer` | Writes tests and validates quality | "test", "cover this", "write tests for", "does this work" |

---

## Delegation Rules

1. The root SoloFounder agent reads the human's request and identifies the primary role needed
2. It delegates to the appropriate sub-agent
3. If a task spans multiple roles (e.g., "build this feature and test it"), it sequences the sub-agents using the `feature-dev` workflow
4. All sub-agents report back to root; root communicates with the human

## Context Passing

Each agent receives:
- The full contents of `memory/runtime/context.md` at session start
- The relevant files from `knowledge/` for their domain
- The RULES.md as a hard constraint layer

## Escalation

If any agent is uncertain about scope or encounters a RULES.md violation condition:
1. Stop
2. Log the uncertainty to `memory/runtime/dailylog.md`
3. Return control to root SoloFounder with a human-readable escalation message
