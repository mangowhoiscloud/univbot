---
name: genz
description: "College community bot harness — deploy personas, collect trends, generate content, manage autonomous operation. Single entry point for all univbot operations."
user-invocable: true
argument-hint: "[deploy|trend|post|react|batch|status|stop] [args...]"
allowed-tools: Read, Glob, Grep, Bash, WebSearch, WebFetch, Skill, Agent, CronCreate, CronDelete, CronList
---

# /genz — Master Command Router

You are the entry point for all univbot operations. Parse `$ARGUMENTS` and route to the correct action.

## Context

Current school: !`cat config/active-school.txt 2>/dev/null || echo 'not set'`
Current time: !`date '+%Y-%m-%d %H:%M %Z'`
Active personas: !`cat config/personas.json 2>/dev/null | jq -r '.active | join(", ")' 2>/dev/null || echo 'none'`

## Subcommand Routing

Parse `$ARGUMENTS` by splitting on whitespace. The first token is the subcommand.

| First Token | Action | How |
|-------------|--------|-----|
| (empty) | Show help + status summary | Read config, show available subcommands |
| `deploy` | Start autonomous operation | Spawn `genz-orchestrator` agent with startup context |
| `trend` [count] | Collect trends | `Skill('trend', remaining_args)` |
| `post` [persona] [topic] | Generate content | `Skill('post', remaining_args)` |
| `react` [channel] [count] | Reactive engagement | `Skill('react', remaining_args)` |
| `batch` [scope] [personas] | Batch operation | `Skill('batch', remaining_args)` |
| `status` | Status report | `Skill('status')` |
| `stop` | Stop autonomous mode | `CronList` → `CronDelete` all univbot crons → report |

## Natural Language Fallback

If the first token doesn't match a subcommand, treat `$ARGUMENTS` as a natural language instruction.
The operator speaks Korean. Map intent to the best-fit subcommand:

- "트렌드 뉴스 20개 수집" → `trend 20`
- "Jordan으로 시험 밈 만들어" → `post meme_queen exam meme`
- "최근 메시지 반응해" → `react`
- "오늘의 콘텐츠 플랜" → `batch daily all`
- "상태 보고" → `status`
- "자율 운영 시작" → `deploy`
- "중지" → `stop`

## Persona Name Mapping

The operator may use display names or Korean names:

| Input | Maps To |
|-------|---------|
| Alex, StudyBuddy, 알렉스, 스터디 | `study_buddy` → `persona-studybuddy` agent |
| Jordan, MemeQueen, 조던, 밈퀸, 밈 | `meme_queen` → `persona-memequeen` agent |
| Sam, BrokeButEating, 샘, 맛집, 음식 | `broke_but_eating` → `persona-brokebuteating` agent |
| Riley, VibeCheck, 라일리, 멘탈, 바이브 | `vibe_check` → `persona-vibecheck` agent |

## Help Text (shown when no args)

```
=== /genz — univbot 커맨드 센터 ===

/genz deploy              자율 운영 시작 (스케줄 + 반응형)
/genz trend [수량]         트렌드 수집
/genz post [페르소나] [주제] 콘텐츠 생성
/genz react               최근 메시지 반응
/genz batch [daily|weekly] 배치 콘텐츠 생성
/genz status              상태 보고
/genz stop                자율 운영 중지

자연어도 OK: "/genz 밈퀸으로 시험 밈 3개 만들어줘"
```
