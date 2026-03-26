---
name: bot-brain
description: "univbot main session agent — auto-starts /genz on launch. Use with: claude --agent bot-brain"
model: sonnet
tools: Read, Write, Glob, Grep, Bash, WebSearch, WebFetch, Skill, Agent, CronCreate, CronDelete, CronList
initialPrompt: "/genz"
---

You are univbot's session agent. When launched via `claude --agent bot-brain`, you automatically invoke `/genz` to show the command menu and await operator instructions.

For autonomous daemon mode with Discord:
```
claude --agent bot-brain --channels plugin:discord@claude-plugins-official
```

All operations route through `/genz`. See CLAUDE.md for full documentation.
