---
name: batch
description: Execute a full content cycle — collect trends, generate content across personas, and prepare for posting. Internal skill — invoked by /genz or orchestrator.
user-invocable: false
argument-hint: [scope: daily|weekly] [personas: all|specific]
---

# Batch Content Operation

Full-cycle content operation: research → plan → generate → review → post.

## Input

- `$1` — scope: "daily" (default) or "weekly"
- `$2` — personas: "all" (default) or comma-separated list

## Process

### Phase 1: Context
- Load school profile and current academic season
- Check what was posted recently (avoid repetition)
- Note day of week, time, upcoming events

### Phase 2: Research
- Run /trend to collect current topics
- Cross-reference with school calendar (midterms? finals? break?)
- Identify 5-10 content opportunities

### Phase 3: Content Plan
- Distribute opportunities across personas:
  - StudyBuddy: 2-3 (study tips, resource shares, exam reminders)
  - MemeQueen: 3-4 (trending meme formats + campus context)
  - BrokeButEating: 1-2 (food deals, seasonal recommendations)
  - VibeCheck: 1-2 (wellness check-ins, stress management)
- Assign optimal posting times per persona's peak hours

### Phase 4: Generate
- Generate all content pieces
- Each in the correct persona voice
- Safety check all pieces

### Phase 5: Review
- Present full plan to operator:
  ```
  === 오늘의 콘텐츠 플랜 ===

  10:00 [StudyBuddy] "midterm tips" — 중간고사 공부법 팁
  12:00 [BrokeButEating] "lunch deals" — 점심 딜 추천
  15:00 [MemeQueen] "procrastination meme" — 미루기 밈
  20:00 [VibeCheck] "evening check-in" — 저녁 안부
  23:00 [MemeQueen] "late night mood" — 심야 밈
  ```
- Operator approves, edits, or removes items

### Phase 6: Execute
- If Discord tools available: post on schedule
- If not: output all content for manual posting with timestamps
