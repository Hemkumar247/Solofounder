<div align="center">

# SoloFounder

### Your AI development team. In a git repo.

[![gitagent spec](https://img.shields.io/badge/spec-gitagent%20v0.1.0-blue)](https://github.com/open-gitagent/gitagent)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![CI](https://github.com/Hemkumar247/solofoundер/actions/workflows/ci.yml/badge.svg)](https://github.com/Hemkumar247/solofoundер/actions)
[![Works with gitclaw](https://img.shields.io/badge/runtime-gitclaw-brightgreen)](https://github.com/open-gitagent/gitclaw)
[![Works with clawless](https://img.shields.io/badge/browser-clawless-purple)](https://github.com/open-gitagent/clawless)

</div>

---

Solo developers don't lack vision. They lack bandwidth.

**SoloFounder** gives you a full engineering team — senior architect, specialist coder, independent reviewer, and QA engineer — all version-controlled, composable, and portable across any runtime. No meetings. No PRs sitting unreviewed for a week. No one reviewing their own code.

Built on the [gitagent](https://github.com/open-gitagent/gitagent) standard. Run it anywhere with [gitclaw](https://github.com/open-gitagent/gitclaw) or in the browser with [clawless](https://github.com/open-gitagent/clawless).

---

## Why SoloFounder?

Every solo developer eventually hits the same wall:

> *"I wrote this feature, reviewed it myself, shipped it — and introduced a subtle race condition I only found three weeks later in production."*

The problem isn't skill. It's **perspective**. No matter how good you are, you cannot objectively review code you just wrote. Senior engineering teams know this — that's why they have Segregation of Duties (SOD): the person who writes the code cannot be the same person who approves it.

SoloFounder brings SOD to solo development. The **Coder agent** cannot review its own PRs. The **QA agent** cannot test its own tests. The **Architect** doesn't write feature code. This isn't bureaucracy — it's the architectural pattern that makes production software reliable.

And because it's a git repo, you get **version control for your AI team**. Roll back a bad prompt. Branch your agent for a new feature. PR improvements to your own team. `git diff` to see exactly how your agent's behaviour changed between versions.

---

## The Team

| Agent | Role | What they do |
|-------|------|-------------|
| 🏛️ **Architect** | System design | Produces a design note before a single line is coded |
| 💻 **Coder** | Implementation | Writes production-quality TypeScript — passes tsc + ESLint before PR |
| 🔍 **Reviewer** | Code review | Structured review with 🚧/⚠️/💡/✅ — cannot review own code |
| 🧪 **QA Engineer** | Quality assurance | Writes tests, produces a QA report — cannot test own code |

The root **SoloFounder** agent reads your request, delegates to the right team member, and sequences them through the workflow. You stay in the loop at the two checkpoints that matter: design approval and merge.

---

## Quick Start

### Run locally with gitclaw

```bash
# Clone the agent
git clone https://github.com/Hemkumar247/solofoundер
cd solofoundер

# Install the gitclaw runtime
npm install -g gitclaw

# Point it at your project and describe the task
gitclaw --dir /path/to/your-app "Add a weekly leaderboard screen"
```

### Run in the browser with clawless

```bash
# Export to a portable system prompt
gitagent export --format system-prompt

# Paste the output into clawless.dev — no server, no setup
```

### Validate the agent spec

```bash
npm install -g gitagent
gitagent validate
gitagent validate --compliance   # SOD policy check
gitagent info
```

---

## How It Works

### Feature Development Flow

```
You: "Add a leaderboard screen"
       │
       ▼
┌─────────────────────────────────────────────────────────────────┐
│                     feature-dev workflow                        │
│                                                                 │
│  1. Architect  ──► Design note in memory/runtime/              │
│       │                                                         │
│  ⏸️  Human approves design                                      │
│       │                                                         │
│  2. Coder  ──► Implements ──► tsc ──► ESLint ──► Opens PR      │
│       │                                                         │
│  3. Reviewer  ──► Structured review ──► APPROVED               │
│  (cannot be the Coder)                                          │
│       │                                                         │
│  4. QA Engineer  ──► Tests ──► QA report ──► QA_PASS           │
│  (cannot be the Coder)                                          │
│       │                                                         │
│  ⏸️  Human merges PR                                            │
└─────────────────────────────────────────────────────────────────┘
```

### Git-Native Memory

Agents commit their state to git. When you start a new session, the agent reads `memory/runtime/context.md` and picks up exactly where you left off — no re-explaining your architecture, no re-answering the same questions.

```
memory/runtime/
├── context.md          # Current project state — read on every bootstrap
├── key-decisions.md    # Architecture decisions with full rationale
├── dailylog.md         # Session-by-session activity log
├── audit.md            # SOD handoff audit trail
└── qa-reports/         # QA reports per feature
```

### Segregation of Duties — Enforced by the Spec

```yaml
# agent.yaml (excerpt)
compliance:
  segregation_of_duties:
    conflicts:
      - [coder, reviewer]      # Coder cannot review own code
      - [coder, qa-engineer]   # Coder cannot test own code
    enforcement: advisory
```

Run `gitagent validate --compliance` to catch SOD violations before deployment.

---

## Repo Structure

```
solofoundер/
│
│   # ── Core Identity ──────────────────────────────────────────────
├── agent.yaml              # Manifest — model, skills, tools, SOD compliance
├── SOUL.md                 # Team identity, values, communication style
│
│   # ── Rules & Duties ───────────────────────────────────────────
├── RULES.md                # Quality gates and hard constraints
├── DUTIES.md               # SOD policy, role boundaries, handoff protocols
├── AGENTS.md               # Delegation logic, fallback instructions
│
│   # ── The Team ─────────────────────────────────────────────────
├── agents/
│   ├── architect/          # agent.yaml + SOUL.md + DUTIES.md
│   ├── coder/              # agent.yaml + SOUL.md + DUTIES.md
│   ├── reviewer/           # agent.yaml + SOUL.md + DUTIES.md
│   └── qa-engineer/        # agent.yaml + SOUL.md + DUTIES.md
│
│   # ── Capabilities ─────────────────────────────────────────────
├── skills/
│   ├── react-native-dev/   # Component patterns, StyleSheet rules, Reanimated
│   ├── firebase-ops/       # Data modeling, security rules, Cloud Functions
│   ├── feature-planning/   # Design note format, acceptance criteria
│   ├── code-review/        # Review checklist, comment format
│   └── documentation/      # README, JSDoc standards
│
│   # ── Tools ────────────────────────────────────────────────────
├── tools/
│   ├── github.yaml         # MCP-compatible: open PRs, post reviews
│   ├── codebase-search.yaml# Search symbols and patterns
│   └── test-runner.yaml    # Run Jest, tsc, ESLint
│
│   # ── Workflows ────────────────────────────────────────────────
├── workflows/
│   ├── feature-dev.yaml    # Design → Code → Review → QA → Merge
│   └── bug-fix.yaml        # Reproduce → Fix → Review → Merge
│
│   # ── Memory & Knowledge ───────────────────────────────────────
├── memory/runtime/         # Persistent agent state (committed to git)
├── knowledge/              # Codebase map, design tokens, Firebase schema
│
│   # ── Lifecycle ────────────────────────────────────────────────
└── hooks/
    ├── bootstrap.md        # Session start: load context, check pending tasks
    └── teardown.md         # Session end: update memory, commit, log next steps
```

---

## Adapting SoloFounder to Your Stack

SoloFounder ships pre-configured for **React Native + Firebase**, but the team is modular.

```bash
# Export your current agent to a system prompt (works with any LLM)
gitagent export --format system-prompt

# Import an existing Claude project config
gitagent import --from claude CLAUDE.md

# Swap out a skill for a different stack
# Remove skills/react-native-dev, add skills/nextjs-dev — done.
```

To adapt to a new stack, replace the skill files in `skills/` and update the domain expertise sections in each sub-agent's `SOUL.md`. The SOD rules, memory system, and workflows require no changes.

---

## Example Session

See [`examples/demo-session.md`](examples/demo-session.md) for a complete walkthrough of:

- A full feature request (leaderboard screen) from human input → architect design → coder PR → reviewer approval → QA pass → merge-ready
- A race condition bug report → QA reproduction test → coder fix → reviewer catch → corrected fix → merge

---

## CI/CD

Every push validates the agent automatically:

```yaml
# .github/workflows/ci.yml
- gitagent validate               # Spec compliance
- gitagent validate --compliance  # SOD policy check
- gitagent export --format system-prompt  # Export smoke test
```

---

## Built with gitagent

SoloFounder is an entry in the [GitAgent Hackathon](https://hackculture.dev/hackathons/gitagent).

The [gitagent](https://github.com/open-gitagent/gitagent) standard makes this possible: a framework-agnostic, git-native format for defining AI agents. Run it with [gitclaw](https://github.com/open-gitagent/gitclaw) locally or [clawless](https://github.com/open-gitagent/clawless) in the browser.

---

## License

MIT
