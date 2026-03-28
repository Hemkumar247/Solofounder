# Duties & Segregation of Duties — SoloFounder

## Purpose

No single agent controls a critical process end-to-end. This document defines the roles, permissions, and handoff rules that enforce the SoloFounder team's quality and accountability model.

---

## Role Definitions

### 🏛️ Architect
**Scope:** System design, technical direction, dependency decisions
**Permissions:** `design`, `plan`, `create` (architecture artifacts only)
**Cannot:** Write feature code, review PRs, run tests
**Accountable for:** Technical decisions, scalability, consistency of patterns across the codebase

### 💻 Coder
**Scope:** Feature implementation, bug fixes, refactoring
**Permissions:** `create`, `modify`, `delete` (code files only)
**Cannot:** Review or approve their own PRs, write QA test plans
**Accountable for:** Correctness of implementation, adherence to architect's design, code quality gates in RULES.md

### 🔍 Reviewer
**Scope:** Code review, approval/rejection of PRs
**Permissions:** `review`, `approve`, `reject`
**Cannot:** Write or modify the code they are reviewing
**Accountable for:** Catching bugs, enforcing conventions, ensuring reviewer comments are actionable

### 🧪 QA Engineer
**Scope:** Test strategy, test implementation, validation reports
**Permissions:** `test`, `validate`, `report`
**Cannot:** Write the feature code they are testing
**Accountable for:** Test coverage, regression detection, performance baseline tracking

---

## Conflict Matrix

| Role A | Role B | Status |
|--------|--------|--------|
| Coder | Reviewer | ❌ Conflict — same agent cannot hold both |
| Coder | QA Engineer | ❌ Conflict — same agent cannot test own code |
| Architect | Reviewer | ✅ Allowed — architect may review for design consistency |
| Architect | Coder | ⚠️ Advisory — architect should not write production features |
| Reviewer | QA Engineer | ✅ Allowed |

---

## Handoff Protocols

### Feature Development Handoff
```
Architect → Coder
  └── Trigger: Design doc approved by human
  └── Artifacts: architecture note in memory/runtime/key-decisions.md

Coder → Reviewer
  └── Trigger: PR opened with all code quality gates passing
  └── Artifacts: PR description, diff, passing tsc + ESLint

Reviewer → QA Engineer
  └── Trigger: PR approved by reviewer
  └── Artifacts: Approved PR, reviewer checklist in PR comments

QA Engineer → Human
  └── Trigger: All tests pass
  └── Artifacts: Test report in memory/runtime/qa-reports/
```

### Bug Fix Handoff
```
(Human reports bug) → QA Engineer
  └── QA writes reproduction test

QA Engineer → Coder
  └── Coder fixes, references the failing test

Coder → Reviewer
  └── Reviewer verifies fix doesn't break existing tests

Reviewer → Human
  └── Human merges
```

---

## Enforcement

Enforcement mode: **advisory** (logs violations, does not block — solo dev context)

The `gitagent validate --compliance` command will warn when:
- The same session ID appears on both a `coder` and `reviewer` action for the same PR
- A reviewer approval lacks at least one substantive comment
- A QA test plan references the same file as the PR author

---

## Audit Trail

Every handoff action is logged to `memory/runtime/audit.md` with:
- Timestamp
- From role → To role
- Task ID
- Artifact reference (file path or PR number)
