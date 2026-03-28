# SoloFounder — Full Documentation

## Architecture Decisions

### Why four agents instead of one?

One agent reviewing its own code is like one developer reviewing their own PR. You're too close to it — you see what you *intended* to write, not what you actually wrote. Four specialised agents with explicit SOD constraints catch more bugs, produce better architecture decisions, and create an audit trail that's useful for debugging later.

### Why git-native memory?

The biggest pain with AI coding assistants is re-explaining context. "Here's my project structure. Here's what we decided last week. Here's why we chose Firebase over Supabase." With git-native memory, the agent commits its understanding of your project to `memory/runtime/` after every session. The next session reads that file first. You never repeat yourself.

Bonus: your agent's memory is version-controlled. If the agent's context gets corrupted or stale, `git log memory/runtime/context.md` shows you exactly how it changed.

### Why advisory SOD enforcement?

Strict SOD (which blocks deployment on violation) is appropriate for regulated industries — FINRA, SEC, Federal Reserve. Solo developers building mobile apps don't need that friction. Advisory mode logs violations and warns you, but never stops you from shipping. You can escalate to `strict` in `agent.yaml` if your use case demands it.

### Why two human checkpoints in the feature-dev workflow?

The first checkpoint (after design) is the high-leverage one. Catching a wrong architectural decision before a single line of code is written saves hours. The second checkpoint (before merge) keeps the human in control of what enters `main`. Everything in between — code, review, QA — runs autonomously.

## Customisation Guide

### Changing the AI model

```yaml
# agent.yaml
model:
  preferred: "openai:gpt-4o"      # OpenAI
  preferred: "google:gemini-2.0-flash"  # Google
  preferred: "claude-opus-4-6"    # Anthropic (default)
```

### Adding a new skill

```bash
mkdir skills/nextjs-dev
cat > skills/nextjs-dev/SKILL.md << 'EOF'
# Skill: Next.js Dev
## Purpose
...
EOF
```

Then register it in `agent.yaml` under `skills:`.

### Adjusting SOD enforcement

```yaml
# agent.yaml
compliance:
  segregation_of_duties:
    enforcement: strict   # blocks deployment on violation
    # enforcement: advisory  # logs warning only
```

### Adding a new sub-agent

```bash
mkdir agents/security-auditor
# Add agent.yaml, SOUL.md, DUTIES.md
# Register in root agent.yaml under agents:
# Add to conflict matrix in DUTIES.md
```

## Running with Different Runtimes

### gitclaw (local CLI)
```bash
gitclaw --dir /your/project "your task description"
gitclaw --dir /your/project --agent architect "design the auth flow"
```

### clawless (browser)
```bash
gitagent export --format system-prompt > system-prompt.txt
# Paste into clawless.dev
```

### Claude Code
```bash
gitagent export --format claude-code > CLAUDE.md
# Claude Code reads CLAUDE.md automatically
```

### OpenAI
```bash
gitagent export --format openai > agents.py
```

### Raw system prompt (any LLM)
```bash
gitagent export --format system-prompt
```
