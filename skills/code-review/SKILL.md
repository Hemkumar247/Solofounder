# Skill: Code Review

## Purpose
Perform structured, actionable code reviews on PRs opened by the coder agent.

## Activation
Activate when a PR is ready for review (triggered by the `feature-dev` or `bug-fix` workflow).

## Review Checklist

### Level 1 — Automated (always check first)
- [ ] TypeScript compiles: `tsc --noEmit` passes
- [ ] ESLint passes on changed files
- [ ] No `console.log` in production paths
- [ ] No hardcoded API keys or secrets

### Level 2 — Logic Review
- [ ] Does the code match the architect's design note?
- [ ] Are all acceptance criteria implemented?
- [ ] Are error paths handled (network failure, auth expiry, null data)?
- [ ] Are async operations awaited? Race conditions considered?

### Level 3 — React Native Specific
- [ ] No inline style objects in render (use `StyleSheet.create`)
- [ ] Lists use `FlatList` or `FlashList`, not `.map()` for > 20 items
- [ ] Heavy computations wrapped in `useMemo`
- [ ] No Firebase listener leaks (all `onValue` calls have cleanup)

### Level 4 — Security
- [ ] Firebase security rules updated to match new data writes
- [ ] User input validated before write
- [ ] PII not logged to console or analytics

## Comment Format
```
🚧 BLOCKING — [File:Line] [Description of issue and how to fix]
⚠️  WARNING — [File:Line] [Description and suggested fix]
💡 SUGGESTION — [File:Line] [Optional improvement]
✅ GOOD — [File:Line] [Calling out a well-done pattern]
```

## Output
- Review comments posted to PR (or returned as text if no GitHub tool available)
- Verdict appended to `memory/runtime/audit.md`
- If APPROVED: notify QA agent to begin test validation
