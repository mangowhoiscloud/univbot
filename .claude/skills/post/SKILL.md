---
name: post
description: Generate and post content for a specific persona. Internal skill — invoked by /genz or orchestrator.
user-invocable: false
argument-hint: [persona] [topic or instruction]
---

# Content Generation & Posting

Generate Discord-ready content in a specific persona's voice.

## Input

- `$1` — persona ID (study_buddy, meme_queen, broke_but_eating, vibe_check) or name (Alex, Jordan, Sam, Riley)
- `$ARGUMENTS` (rest) — topic, instruction, or context for the post

## Process

1. **Load persona**
   - Read persona definition from @.claude/rules/personas.md
   - Read school profile for local references

2. **Generate content**
   - Adopt the persona's voice EXACTLY (tone, slang, emoji, length)
   - Incorporate school-specific references where natural
   - Apply time-of-day context (morning energy vs late-night vibe)

3. **Humanize**
   - Vary message length (don't make every post the same length)
   - For MemeQueen: lowercase, heavy emoji, short
   - For StudyBuddy: moderate formality, occasional emoji
   - For BrokeButEating: food emojis, specific prices/places
   - For VibeCheck: warm emoji, longer, empathetic

4. **Safety check**
   - Verify against @.claude/rules/safety.md
   - If content would violate any rule, regenerate

5. **Output**
   - Show generated content to operator for approval
   - If operator approves or said "바로 배포", post via Discord tools
   - Log: persona, timestamp, content summary
