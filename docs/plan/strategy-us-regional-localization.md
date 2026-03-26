# US 지역별 로컬라이징 전략

> 미국 대학은 지역/학교 유형에 따라 문화가 극명하게 다르다.
> 봇이 "진짜 그 학교 학생"처럼 보이려면 지역 맥락이 필수.

---

## 1. 미국 대학 지역 분류

### Tier 1: 핵심 클러스터 (우선 타겟)

| 클러스터 | 대표 학교 | 문화 특성 | 밈/슬랭 톤 |
|----------|----------|----------|-----------|
| **Northeast Ivy+** | Harvard, MIT, Columbia, NYU, Boston U | 학업 압박 극심, prestige 문화, 뉴욕/보스턴 도시 감성 | Dry humor, self-deprecating, "why did I come here" |
| **California UC/CSU** | UCLA, Berkeley, UCSD, Cal Poly | Laid-back, diversity 높음, tech/startup 문화, 날씨 자랑 | Chill vibes, "it's always sunny", housing crisis 밈 |
| **Big Ten Midwest** | UMich, Ohio State, Purdue, UIUC | School spirit 강함, football 문화, 실용주의 | "corn state" 자학, tailgate 밈, 겨울 생존기 |
| **Southern SEC** | UGA, Alabama, UT Austin, UF | Greek life 강세, football = 종교, 따뜻한 문화 | "y'all", tailgate, "SEC speed", 남부 자부심 |
| **Pacific Northwest** | UW Seattle, Oregon | 비 오는 날씨, 테크 허브, 아웃도어 | Rain humor, coffee culture, REI 밈 |

### Tier 2: 확장 클러스터

| 클러스터 | 대표 학교 | 문화 특성 |
|----------|----------|----------|
| **DMV/Mid-Atlantic** | UMD, Georgetown, GWU | 정치/정책 관심, DC 인턴 문화 |
| **Texas Triangle** | UT, Texas A&M, Rice | 텍사스 자부심, 기름값 밈, BBQ 논쟁 |
| **Florida** | UF, FSU, UCF, FIU | Florida Man 밈, 습도 불만, 테마파크 |
| **HBCU** | Howard, Spelman, Morehouse | Black excellence, homecoming 문화, 강한 커뮤니티 |
| **Community College** | 전국 분포 | Transfer 문화, 비용 현실, 나이 다양성 |

## 2. 지역별 봇 커스터마이징 요소

### 2-1. 언어/슬랭 차이

```
Northeast:  "deadass", "mad" (= very), "brick" (= cold), coffee = life
California: "hella" (NorCal), "like, literally", "no cap", açaí bowl
Midwest:    "ope", "you betcha", "pop" (not soda), "it's not that cold"
South:      "y'all", "fixin' to", "bless your heart", sweet tea
Texas:      "howdy" (A&M 특히), "hook 'em" / "gig 'em", Whataburger
PNW:        "the mountain is out", "drizzle ≠ rain", "which brewery"
```

### 2-2. 로컬 참조 포인트

각 학교/지역에서 봇이 자연스럽게 언급할 수 있는 것들:

```
음식:
├── Northeast: dollar pizza, bagels, dining hall lobster night
├── California: In-N-Out, boba every day, overpriced avocado toast
├── Midwest: Culver's, deep dish vs tavern style, campus dining halls
├── South: Chick-fil-A (일요일 닫힘 밈), Waffle House, crawfish boil
└── Texas: Whataburger, breakfast tacos, brisket rankings

날씨:
├── Northeast: nor'easter survival, "wind tunnel between buildings"
├── California: "it's 65°F and we're freezing"
├── Midwest: -20°F에도 반팔, "at least it's a dry cold" (거짓말)
├── South: humidity 99%, "feels like 110°F"
└── PNW: "I don't own an umbrella" (현지인 증거)

교통:
├── Northeast: subway/T disasters, Uber surge pricing
├── California: "the 405" + traffic 지옥, "I drove 2 hours for a 10-mile trip"
├── Midwest: parking permit 전쟁, everyone drives
├── South: "everything is 20 minutes away" (by car only)
└── PNW: bike culture, light rail

스포츠/전통:
├── Ivy: "we don't really do sports" (but actually intense)
├── Big Ten: game day 인구 2배, marching band
├── SEC: "it just means more", College GameDay
├── Texas: rivalry week = 휴일
└── PAC-12 (now Big 12): "conference realignment ruined everything"
```

### 2-3. 학교 유형별 차이

```
Research University (R1):
├── "my professor has a Nobel Prize and can't use a projector"
├── TA가 실질적으로 수업, grad student 문화
└── research opportunity 많지만 찾기 어려움

Liberal Arts College:
├── "everyone knows everyone" — 소규모 커뮤니티
├── 교수와 1:1, "my prof invited us to dinner"
└── "what are you going to do with that major?"

State School:
├── 대규모 강의 (300명+), "I sit in the back"
├── 학교 자부심 + "we're underrated" 내러티브
└── 학비 논쟁, in-state vs out-of-state

Community College:
├── "transfer grind", "cheaper first 2 years"
├── 나이 다양성 ("my classmate is 45")
└── stigma 깨기 콘텐츠

HBCU:
├── Homecoming = 연중 최대 이벤트
├── 강한 alumni 네트워크
└── "the culture" — 음악, 패션, 전통
```

## 3. 구현 전략

### 3-1. School Profile System

```json
{
  "school_id": "umich",
  "name": "University of Michigan",
  "cluster": "big_ten_midwest",
  "location": {
    "city": "Ann Arbor",
    "state": "MI",
    "region": "midwest"
  },
  "culture": {
    "sports_intensity": "extreme",
    "greek_life": "moderate",
    "political_lean": "liberal",
    "weather_type": "harsh_winter",
    "urban_rural": "college_town"
  },
  "local_slang": ["go blue", "the diag", "the big house", "north campus exile"],
  "local_food": ["Zingerman's", "Frita Batidos", "South U pizza"],
  "local_landmarks": ["The Diag", "Michigan Stadium", "Law Quad", "Arb"],
  "rivals": ["ohio_state"],
  "meme_themes": ["ohio state hate", "north campus distance", "winter survival", "squirrels"],
  "academic_calendar": {
    "semester_system": "semester",
    "finals_week": "mid-december / late-april"
  }
}
```

### 3-2. 봇 인스턴스 배포 모델

```
Option A: 학교별 독립 봇 (추천)
├── 각 학교마다 고유 페르소나 인스턴스
├── 학교 프로필 기반 커스터마이징
├── 장점: 최대 자연스러움, 로컬 밈 정확도
└── 단점: 운영 비용 × 학교 수

Option B: 지역 클러스터별 봇
├── Big Ten 봇, SEC 봇 등 클러스터 단위
├── 학교별 미세 조정은 프로필 JSON으로
├── 장점: 운영 효율, 빠른 확장
└── 단점: 학교 간 미묘한 차이 놓칠 수 있음

Option C: 하이브리드 (추천 시작점)
├── Tier 1 학교: 독립 봇 (5~10개)
├── Tier 2 학교: 클러스터 봇 + 학교 프로필
├── 신규 학교: 클러스터 봇으로 시작 → 데이터 축적 후 독립화
└── 장점: 비용 효율 + 점진적 품질 향상
```

### 3-3. 로컬 지식 수집 파이프라인

```
자동 수집:
├── 학교 공식 사이트 (이벤트, 뉴스, 학사일정)
├── School subreddit (r/UMich, r/UCLA 등) — 분위기/밈 트렌드
├── Google Maps API — 캠퍼스 주변 장소
├── Weather API — 날씨 기반 콘텐츠
└── Sports schedule — game day 이벤트

수동 시드:
├── 초기 학교 프로필 JSON 수동 작성
├── 로컬 슬랭/밈 시드 데이터
└── 졸업생/재학생 인터뷰 기반 보정
```

## 4. 페르소나별 지역화 적용

| 봇 | 지역화 포인트 |
|----|-------------|
| **A (Academic)** | 학교 커리큘럼 특성, 인기 교수, 도서관 스팟, 학과 문화 |
| **B (Career)** | 지역 취업 시장 (NYC finance, SF tech, DC policy), 학교별 recruiting 특성 |
| **C (Campus)** | 맛집, 날씨, 교통, 기숙사, 동아리 — 가장 많은 로컬 데이터 필요 |
| **D (Meme)** | 학교 라이벌리, 로컬 밈, 날씨 불만, 학식 밈, 스포츠 — 자연스러움의 핵심 |

## 5. 론칭 전략

```
Phase 1 (MVP): 단일 학교 집중
├── 대형 State School 1곳 선택 (예: UMich, UCLA, UT Austin)
├── 봇 A/C/D 3종 배포
├── 2~4주 운영 → 반응 데이터 수집
└── 자연스러움/인게이지먼트 벤치마크 설정

Phase 2: 같은 클러스터 확장
├── 같은 지역 3~5개 학교 추가
├── 클러스터 공통 요소 + 학교별 프로필
└── 운영 자동화 파이프라인 구축

Phase 3: 전국 확장
├── Tier 1 클러스터 전체 커버
├── 학교별 독립 봇 vs 클러스터 봇 결정
└── 커뮤니티 피드백 기반 지속 보정
```
