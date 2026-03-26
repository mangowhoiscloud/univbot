# univbot

AI-powered college community bot harness for Claude Code.

Deploy autonomous personas (StudyBuddy, MemeQueen, BrokeButEating, VibeCheck) on US college Discord servers — all controlled through a single `/genz` command.

## Install

### Option A: Plugin Install (Recommended)

In any Claude Code session:

```
/plugin marketplace add mangowhoiscloud/univbot
/plugin install univbot
```

Once installed, `/genz` is available in every project.

### Option B: Clone & Run

```bash
git clone https://github.com/mangowhoiscloud/univbot.git
cd univbot
claude
```

`/genz` is available within the project directory.

### Option C: Auto-start Mode

```bash
git clone https://github.com/mangowhoiscloud/univbot.git
cd univbot
claude --agent bot-brain
```

The `bot-brain` agent auto-runs `/genz` on session start via `initialPrompt`.

## Usage

```
/genz                     Show command menu
/genz deploy              Start autonomous operation
/genz trend [count]       Collect trending topics
/genz post [persona] [topic]  Generate persona content
/genz react               React to recent messages
/genz batch               Daily content batch
/genz status              Status report
/genz stop                Stop autonomous mode
```

Natural language works too:

```
/genz 밈퀸으로 시험 밈 3개 만들어줘
/genz collect 20 trending topics and post them
```

## Personas

| Name | Role | Vibe |
|------|------|------|
| **StudyBuddy** (Alex) | Academic helper | Patient, nerdy, shares free resources |
| **MemeQueen** (Jordan) | Campus meme machine | Unhinged Gen Z, all lowercase, 💀😭🫠🤡 |
| **BrokeButEating** (Sam) | Budget food expert | Knows every $5 meal within 5 miles |
| **VibeCheck** (Riley) | Mental wellness | Warm, validating, shares crisis resources |

## Architecture

```
/genz (master skill)
  ├─ deploy → genz-orchestrator (sonnet)
  │             ├─ CronCreate (autonomous schedule)
  │             ├─ persona-studybuddy (haiku)
  │             ├─ persona-memequeen (haiku)
  │             ├─ persona-brokebuteating (haiku)
  │             └─ persona-vibecheck (haiku)
  ├─ trend/post/react/batch → internal skills
  └─ status/stop → direct execution
```

Zero code. Markdown + JSON only. Claude Code is the runtime.

## School Profiles

Pre-configured: UMich, UCLA. Add your own:

```bash
# Copy template
cp config/school-profiles/umich.json config/school-profiles/your-school.json

# Edit with your school's info
# Then set active:
echo "your-school" > config/active-school.txt
```

## Execution Modes

```bash
# Interactive (dev/test)
cd univbot && claude

# Auto-start
claude --agent bot-brain

# Daemon with Discord
claude --agent bot-brain --channels plugin:discord@claude-plugins-official

# Headless (CI/cron)
claude -p "/genz batch daily all" --agent bot-brain
```

## How It Works

univbot uses Claude Code itself as the bot runtime — no Python, no Node.js, no external servers. The entire system is markdown + JSON:

| Layer | Files | Role |
|-------|-------|------|
| **Entry point** | `genz/SKILL.md` | `/genz` slash command with subcommand routing |
| **Orchestrator** | `genz-orchestrator.md` | Spawns persona agents, manages schedules via CronCreate |
| **Personas** | `persona-*.md` (x4) | Autonomous agents with distinct voices (haiku model) |
| **Rules** | `personas.md`, `safety.md` | Voice definitions + hard safety constraints |
| **Hooks** | `settings.json` | SessionStart context injection, activity logging |
| **Config** | `personas.json`, `school-profiles/` | Per-persona settings, school-specific local data |

### Trigger Types

| Type | Example | How |
|------|---------|-----|
| **Slash command** | `/genz deploy` | Direct skill invocation |
| **Natural language** | `/genz 밈퀸으로 시험 밈 만들어` | NL parsing → subcommand routing |
| **Hook** | SessionStart | Auto-injects school/time context every session |
| **initialPrompt** | `claude --agent bot-brain` | Auto-runs `/genz` on session start |
| **CronCreate** | After `/genz deploy` | Periodic autonomous content generation |
| **Headless** | `claude -p "/genz batch"` | CI/cron one-shot execution |

## Contributing

1. Fork the repo
2. Add a school profile (`config/school-profiles/your-school.json`)
3. Or add a new persona agent (`.claude/agents/persona-yourbot.md`)
4. Submit a PR

## License

MIT
