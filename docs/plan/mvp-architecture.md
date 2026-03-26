# MVP Architecture — univbot (Thin Harness)

> Claude Code 자체를 봇 런타임으로 사용하는 얇은 하네스 아키텍처
> 별도 Python 서버 없음. CLAUDE.md + skills + hooks + agents = 전부.

---

## 1. 핵심 전환

| 이전 (폐기) | 현재 (채택) |
|-------------|-------------|
| Claude Code Plugin으로 마켓플레이스 배포 | Claude Code를 **외부 하네스**로 직접 사용 |
| discord.py + MCP 서버 번들 | **Claude Code Channels** (Discord 공식 플러그인) |
| 별도 Python 봇 런타임 | **Claude Code 자체가 LLM 브레인** |
| 커스텀 classifier/humanizer/safety 코드 | **CLAUDE.md + rules/ 마크다운**으로 전부 정의 |
| plugin.json 배포 | **로컬 실행** (headless, daemon, 또는 interactive) |

## 2. 아키텍처

```
┌─────────────────────────────────────────────┐
│                    Operator                   │
│     "트렌드 뉴스 20개 수집해서 배포해줘"         │
└──────────────────┬──────────────────────────┘
                   │ 자연어
                   ▼
┌─────────────────────────────────────────────┐
│              Claude Code (harness)            │
│                                              │
│  CLAUDE.md ─── 마스터 브레인 (라우팅, 원칙)      │
│  rules/    ─── 페르소나 정의 + 안전 규칙         │
│  skills/   ─── /trend /post /react /batch     │
│  agents/   ─── bot-brain (--agent 모드)        │
│  settings  ─── 권한 + hooks                    │
│                                              │
│  Built-in Tools:                             │
│  ├─ WebSearch (트렌드/뉴스 수집)                │
│  ├─ WebFetch (콘텐츠 크롤링)                    │
│  ├─ Read/Write (설정, 로그)                     │
│  └─ Bash (유틸리티)                            │
│                                              │
│  External:                                   │
│  └─ Discord Channel Plugin (양방향 I/O)        │
└──────────────────┬──────────────────────────┘
                   │ channel tools
                   ▼
┌─────────────────────────────────────────────┐
│              Discord Server                   │
│  #general  #study  #food  #memes  #wellness  │
│  학생 유저들 + univbot 페르소나들                 │
└─────────────────────────────────────────────┘
```

## 3. 실행 모드 3가지

### Mode A: Interactive (개발/테스트)
```bash
cd /Users/mango/workspace/univbot
claude
# → CLAUDE.md 자동 로드
# → 자연어로 명령: "MemeQueen으로 시험 기간 밈 5개 만들어줘"
# → Claude가 WebSearch로 트렌드 수집 → 밈 생성 → 출력
```

### Mode B: Daemon (운영 — Discord 실시간 연결)
```bash
claude --agent bot-brain --channels plugin:discord@claude-plugins-official
# → Discord 메시지 실시간 수신
# → bot-brain 에이전트가 페르소나 라우팅 + 응답 생성
# → channel tool로 Discord에 자동 응답
```

### Mode C: Headless (자동화 — cron/CI)
```bash
# 일일 배치 콘텐츠 생성
claude -p "오늘의 배치 콘텐츠 생성해서 배포해" \
  --agent bot-brain \
  --allowedTools "Read,Write,Glob,WebSearch,WebFetch,Bash,Skill"

# 파이프 입력
echo "UCLA 캠퍼스 뉴스 기반으로 StudyBuddy 포스트 3개" | claude -p --agent bot-brain
```

## 4. 자연어 쿼리 → 실행 흐름

```
오퍼레이터: "오늘의 트렌드에 맞는 뉴스들을 20개 수집해서 배포해줘"

1. Claude Code 수신 (CLAUDE.md 컨텍스트 로드됨)
2. 의도 파싱: trend collection + content generation + posting
3. /trend 스킬 실행:
   ├─ config/active-school.txt → "umich"
   ├─ config/school-profiles/umich.json → 로컬 컨텍스트 로드
   ├─ WebSearch "University of Michigan trending March 2026"
   ├─ WebSearch "college students trending memes this week"
   ├─ WebSearch "Ann Arbor events this week"
   └─ 20개 트렌드 수집 + 태깅 (topic, persona fit)
4. 오퍼레이터에게 리스트 보고 (한국어)
5. "전부 배포해" → /post 스킬 × 20:
   ├─ 각 트렌드별 최적 페르소나 선택
   ├─ 페르소나 보이스로 콘텐츠 생성
   ├─ safety rules 검증
   └─ Discord channel tool로 게시 (또는 텍스트 출력)
```

## 5. 파일 구조 (최종)

```
univbot/
├── CLAUDE.md                          # 마스터 브레인 (라우팅, 원칙)
├── .claude/
│   ├── settings.json                  # 권한 + hooks
│   ├── rules/
│   │   ├── personas.md                # 4개 페르소나 보이스 정의
│   │   └── safety.md                  # 안전 규칙 (최우선)
│   ├── skills/
│   │   ├── trend/SKILL.md             # /trend — 트렌드 수집
│   │   ├── post/SKILL.md              # /post — 콘텐츠 생성+게시
│   │   ├── react/SKILL.md             # /react — 반응형 참여
│   │   ├── batch/SKILL.md             # /batch — 배치 오퍼레이션
│   │   └── status/SKILL.md            # /status — 상태 보고
│   └── agents/
│       └── bot-brain.md               # --agent 모드용 에이전트
├── config/
│   ├── active-school.txt              # 현재 활성 학교
│   ├── personas.json                  # 페르소나 수치 설정
│   └── school-profiles/
│       ├── umich.json                 # UMich 로컬 데이터
│       └── ucla.json                  # UCLA 로컬 데이터
├── logs/
│   └── .gitkeep
└── docs/
    └── plan/                          # 기획 문서
```

**총 파일 수: ~20개 (마크다운 + JSON만. 코드 0줄.)**

## 6. bkit-starter에서 추출한 구조적 레퍼런스

| bkit 요소 | univbot 적용 |
|-----------|-------------|
| skills/{name}/SKILL.md | .claude/skills/{name}/SKILL.md — 동일 포맷 |
| commands/{name}.md | skills로 통합 (user-invocable: true) |
| .claude/settings.json 권한 | 동일 — 필요 도구만 allow |
| hooks/hooks.json | .claude/settings.json 내 hooks 섹션 |
| GUIDE.md 레벨 체계 | 참고만 — 우리는 단일 레벨 |
| Stage 0~N 패턴 | 각 skill 내 Phase/Process 구조로 채택 |

## 7. 커스텀 코드 vs 프롬프트 엔지니어링

| 기존 (코드) | 현재 (프롬프트) |
|-------------|----------------|
| classifier.py (regex 기반) | CLAUDE.md routing rules (LLM이 직접 분류) |
| humanizer.py (규칙 기반) | personas.md 보이스 정의 (LLM이 직접 어댑트) |
| safety.py (regex 필터) | safety.md 규칙 (LLM이 직접 판단) |
| bot.py (discord.py) | Claude Code Channels (공식 플러그인) |
| server.py (MCP 서버) | 불필요 — Claude Code 내장 도구 사용 |

**핵심 인사이트**: LLM 자체가 classifier이자 humanizer이자 safety filter.
규칙을 마크다운으로 정의하면 코드가 필요 없다.
