---
name: trend
description: Collect trending topics and news relevant to US college students. Internal skill — invoked by /genz or orchestrator.
user-invocable: false
argument-hint: [count] [topic-filter]
---

# Trend Collection

Collect trending topics relevant to US college students and prepare them for persona-based content.

## Input

- `$ARGUMENTS` — optional: number of items and/or topic filter
- Default: 20 items, no filter

## Process

1. **Determine context**
   - Read `config/active-school.txt` for current school
   - Read school profile for local context (location, academic calendar)
   - Check current date/time for seasonal relevance

2. **Collect trends** via WebSearch
   - Campus news: "[school name] news this week"
   - Academic: "college students trending topics {month} {year}"
   - Cultural: "gen z trending memes this week"
   - Local: "[city] events this week college"
   - If topic filter provided, focus searches accordingly

3. **Curate and tag**
   - For each trend, tag with: `academic | food | humor | wellness | career | campus_life | culture`
   - Tag persona fit: which persona(s) could use this?
   - Rate relevance (1-5) for the specific school context

4. **Output**
   - Present curated list to operator (Korean)
   - Format: numbered list with trend, tag, persona fit, relevance score
   - Ask operator which to proceed with, or auto-proceed if instructed

5. **Optional: Generate content**
   - If operator says "배포해" or "포스팅해", generate content for each selected trend
   - Use the tagged persona's voice
   - Apply school-specific references
   - Post via Discord tools if available, otherwise output for manual posting
