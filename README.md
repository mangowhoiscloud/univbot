# univbot

AI-powered college community bot harness for Claude Code.

Deploy autonomous personas (StudyBuddy, MemeQueen, BrokeButEating, VibeCheck) on US college Discord servers — all controlled through a single `/genz` command.

## Install

```bash
# Add the marketplace
/plugin marketplace add mangowhoiscloud/univbot

# Install the plugin
/plugin install univbot
```

Or clone directly:

```bash
git clone https://github.com/mangowhoiscloud/univbot.git
cd univbot
claude
```

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

## License

MIT
