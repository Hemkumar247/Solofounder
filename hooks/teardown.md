# Teardown — Session End Instructions

Every agent runs this before ending a session.

---

## Step 1: Update context.md

Append or update the following sections in `memory/runtime/context.md`:
- "Recent Decisions" — any architectural choices made this session
- "Current Sprint" — check off completed items, add new ones
- "Known Issues" — log any problems discovered

## Step 2: Write a Daily Log Entry

Append to `memory/runtime/dailylog.md`:
```
### [YYYY-MM-DD HH:MM] — [Your role]
Task: [what you worked on]
Status: completed | blocked | escalated
Notes: [what happened, what was learned]
Next: [what the next agent/session should do]
```

## Step 3: Update Audit Trail (if handoff occurred)

If you handed off to another agent during this session, append a row to `memory/runtime/audit.md`.

## Step 4: Commit Memory Files

The following files should be committed with message "chore(memory): session update [date]":
- `memory/runtime/context.md`
- `memory/runtime/dailylog.md`
- `memory/runtime/audit.md`
- `memory/runtime/key-decisions.md` (if updated)
- `memory/runtime/qa-reports/` (if any new reports)

---

## Final Message to Human

End every session with:
```
Session complete. Memory updated and committed.
Next recommended action: [one clear sentence]
```
