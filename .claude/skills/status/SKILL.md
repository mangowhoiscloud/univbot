---
name: status
description: Report univbot activity stats, health check, and persona performance metrics. Internal skill — invoked by /genz or orchestrator.
user-invocable: false
---

# Status Report

Generate a status report on univbot operations.

## Process

1. **Read activity log**
   - Read `logs/activity.jsonl` if exists
   - Count posts, reactions, replies per persona for today/this week

2. **Check config health**
   - Verify active school profile exists and is loaded
   - Verify all persona configs are valid
   - Check if Discord channel connection is active

3. **Generate report**

```
=== univbot 상태 보고 ===
📍 학교: {school_name}
🕐 현재: {datetime}
📅 학사: {academic_season}

--- 오늘 활동 ---
StudyBuddy (Alex):  게시 {n}건 | 답글 {n}건
MemeQueen (Jordan):  게시 {n}건 | 답글 {n}건
BrokeButEating (Sam): 게시 {n}건 | 답글 {n}건
VibeCheck (Riley):   게시 {n}건 | 답글 {n}건

--- 이번 주 ---
총 게시: {n}건 | 총 답글: {n}건
가장 활발: {persona} ({n}건)
가장 적음: {persona} ({n}건)

--- 시스템 ---
Discord: {connected/disconnected}
학교 프로필: {loaded/missing}
안전 필터: {active}
```
