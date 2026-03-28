# Demo Session — SoloFounder in Action

This shows a complete feature development session using SoloFounder with gitclaw.

---

## Setup

```bash
# Clone the agent
git clone https://github.com/YOUR_HANDLE/solofoundер
cd solofoundер

# Install gitclaw runtime
npm install -g gitclaw

# Set your API key
export ANTHROPIC_API_KEY=your_key_here

# Run the agent on your project
gitclaw --dir /path/to/your-react-native-app "Help me build a leaderboard screen"
```

---

## Example Interaction 1 — Feature Request

**Human:**
> Add a leaderboard screen that shows the top 10 territory holders this week

**SoloFounder (root):**
> This spans design, implementation, and QA. Running the `feature-dev` workflow.
> Delegating to Architect first.

**Architect:**
> Architect ready. Context loaded. 0 pending items.
>
> **Design Note: Weekly Leaderboard Screen**
> **Date:** 2026-03-28 | **Status:** Draft
>
> **Problem:** Users have no visibility into how they rank against others, reducing competitive motivation.
>
> **Proposed Approach:**
> - New screen: `LeaderboardScreen.tsx`
> - New hook: `useLeaderboard.ts` — subscribes to `leaderboard/byTerritories` in Firebase RTDB
> - Existing `leaderboard` node in DB is already written by Cloud Functions — no schema change needed
> - Display: Animated `FlatList` of top 10, highlight current user's row, weekly reset indicator
>
> **Acceptance Criteria:**
> - [ ] Shows top 10 users by territory count for the current week
> - [ ] Current user's rank is visible even if outside top 10
> - [ ] Animates in on first render (stagger entrance, spring physics)
> - [ ] Empty state when no data
> - [ ] Pulls to refresh
>
> ⚠️  Risk: "Weekly" leaderboard requires a server-side timestamp filter. Cloud Function needs updating.
>
> Saving to `memory/runtime/key-decisions.md`. **Awaiting human approval.**

**Human:**
> Looks good. Go ahead.

**Coder:**
> Coder ready. Design note loaded.
>
> Implementing `LeaderboardScreen.tsx`, `useLeaderboard.ts`, and updating the Cloud Function for weekly filtering.
> Running tsc and ESLint now...
> ✅ Zero TypeScript errors. ✅ Zero ESLint warnings.
>
> PR opened: `feat/leaderboard-screen` → `main`
> PR description: [What changed, acceptance criteria checklist, screenshots]

**Reviewer:**
> Reviewing `feat/leaderboard-screen`...
>
> ✅ GOOD — `src/hooks/useLeaderboard.ts:12` — Firebase listener properly cleaned up in useEffect return
> ✅ GOOD — FlatList used correctly with keyExtractor and getItemLayout
> ⚠️  WARNING — `src/screens/LeaderboardScreen.tsx:67` — Current user rank fetch is a separate DB read that could be batched with the top-10 query. Not blocking, but consider for v2.
> 💡 SUGGESTION — `functions/src/leaderboard.ts:23` — Add index hint comment for the weekly query
>
> **APPROVED** — Handing off to QA.

**QA Engineer:**
> Writing tests for `feat/leaderboard-screen`...
>
> Tests written:
> - ✅ Renders top 10 users correctly
> - ✅ Highlights current user row
> - ✅ Shows empty state when data is null
> - ✅ Pull-to-refresh triggers re-fetch
> - ✅ Listener cleanup on unmount (no memory leak)
>
> Full test suite: 47 passing, 0 failing. Coverage delta: +4.2%
>
> QA Report saved to `memory/runtime/qa-reports/leaderboard-screen.md`
> **QA_PASS** ✅

**SoloFounder (root):**
> 🎉 Feature ready to merge.
> All gates passed: Design ✅ | Code ✅ | Review ✅ | QA ✅
> Merge `feat/leaderboard-screen` when ready.

---

## Example Interaction 2 — Bug Report

**Human:**
> The app crashes when two users claim the same territory at the same time

**SoloFounder (root):**
> Race condition bug. Severity: high. Running `bug-fix` workflow.

**QA Engineer:**
> Writing reproduction test...
> `__tests__/integration/territory-claim-race.test.ts` — **FAILING** as expected.
> The test simulates two concurrent writes to the same territory node.
> Firebase last-write-wins by default — no conflict resolution.

**Architect:**
> Root cause: multi-path write in `claimTerritory()` is not atomic in the race condition.
> Fix: Use Firebase Realtime Database **transactions** (`runTransaction`) instead of `update()` for territory claims.
> This guarantees only one claim succeeds if two arrive simultaneously.

**Coder:**
> Refactoring `services/territories.ts#claimTerritory` to use `runTransaction`.
> PR opened: `fix/territory-claim-race`

**Reviewer:**
> 🚧 BLOCKING — Transaction should include the `userTerritories` fan-out update inside the transaction, not after. Currently if the transaction succeeds but the fan-out fails, the data is inconsistent.
> **CHANGES_REQUESTED**

**Coder:**
> Fixed. Both writes now inside a single transaction.
> PR updated.

**Reviewer:**
> APPROVED ✅

**SoloFounder:**
> Bug fix ready. 1 revision cycle. Merge `fix/territory-claim-race`.

---

## Running with clawless (Browser, No Server)

```bash
# Export to system prompt and paste into clawless
gitagent export --format system-prompt
# → Copy output into clawless.dev and run in-browser
```
