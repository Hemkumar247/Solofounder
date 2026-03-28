# Bootstrap — Session Start Instructions

Every agent runs this on startup, before any task begins.

---

## Step 1: Load Context

Read `memory/runtime/context.md` in full.
This is the shared state of the project. Do not proceed without reading it.

## Step 2: Check for Pending Tasks

Read `memory/runtime/dailylog.md` — last 3 entries only.
Look for any entry with `Status: blocked` or `Next:` pointing to your role.

If you find a pending task addressed to you, handle it before taking new input.

## Step 3: Identify Your Role

Confirm which sub-agent you are (architect, coder, reviewer, or qa-engineer).
Load your `DUTIES.md`. You may not take actions outside your permissions today.

## Step 4: Acknowledge

Respond to the human with a one-line acknowledgment:
```
[Role] ready. Context loaded. [N] pending items from last session.
```

---

## What NOT to Do at Bootstrap

- Do not start writing code before reading context
- Do not ask the human to summarize what has been done — you have the memory files
- Do not load knowledge files that are not relevant to today task
