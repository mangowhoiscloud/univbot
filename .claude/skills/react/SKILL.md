---
name: react
description: Read recent Discord messages and generate persona-appropriate reactions. Internal skill — invoked by /genz or orchestrator.
user-invocable: false
argument-hint: [channel] [count]
---

# Reactive Engagement

Read recent Discord channel messages and generate natural responses from appropriate personas.

## Input

- `$1` — channel name or ID (optional, defaults to main channel)
- `$2` — number of messages to review (optional, default 30)

## Process

1. **Read channel**
   - Fetch recent messages via Discord tools
   - Skip bot's own messages and system messages

2. **Classify each message**
   - Topic: academic, food, humor, wellness, career, campus_life, general
   - Intent: question, rant, sharing, seeking_help, humor, discussion
   - Sentiment: positive, negative, neutral
   - Engagement potential: high, medium, low

3. **Select messages to respond to**
   - NOT every message — aim for 40-60% engagement rate
   - Prioritize: questions > seeking_help > high-engagement humor > sharing
   - Skip: low-effort posts, already-answered questions, controversial topics

4. **Assign personas**
   - Match topic to persona specialty
   - If multiple personas could respond, pick the best fit (don't dogpile)
   - Max 1 persona responds per message

5. **Generate responses**
   - Each response in the assigned persona's voice
   - Short, chat-appropriate (1-3 sentences)
   - Reference previous messages in thread for context
   - Natural variation in response style

6. **Output**
   - Present responses to operator: [persona] → [target message] → [response]
   - Operator can approve all, edit, or selective-approve
   - Post approved responses with staggered timing (note in output)

## Timing Note

When posting multiple responses, suggest staggered timing:
- First response: immediate
- Subsequent: 2-10 minute gaps
- This prevents "bot spam" appearance
