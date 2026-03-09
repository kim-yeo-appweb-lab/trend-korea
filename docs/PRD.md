# 트렌드 코리아 통합 PRD

> 상태: 확정 | 작성일: 2026-03-09 | 최종 검증: 2026-03-09 (코드베이스 대조 완료)

---

## 1. 제품 정의 (WHY)

### 1.0 프로젝트 구조

본 서비스는 3개의 독립 프로젝트로 구성된다:

| 디렉토리 | 역할 | 기술 스택 | 상세 PRD |
|----------|------|----------|----------|
| `trend-korea-api/` | 백엔드 API | FastAPI + SQLAlchemy 2.0 + PostgreSQL | [`trend-korea-api/docs/PRD.md`](../trend-korea-api/docs/PRD.md) |
| `trend-korea-app/` | 사용자 프론트엔드 | Next.js 16 + React 19 + TypeScript | [`trend-korea-app/docs/PRD.md`](../trend-korea-app/docs/PRD.md) |
| `trend-korea-admin/` | 어드민 풀스택 (신규) | Next.js + Prisma + shadcn/ui | [`trend-korea-admin/docs/PRD.md`](../trend-korea-admin/docs/PRD.md) |

### 1.1 한 줄 정의

대한민국에서 발생하는 사건, 법률 개정, 사회적 특이점을 시간순으로 기록하고, 이슈가 된 사건을 지속 추적하여 제공하는 정보 아카이브 서비스

### 1.2 해결하는 문제

- 뉴스가 여러 매체에 흩어져 있어 하나의 사건을 시간순으로 파악하기 어렵다
- 한 번 보도된 사건의 후속 경과를 추적하려면 직접 검색해야 한다
- 사건에 대한 맥락 있는 토론 공간이 부족하다

### 1.3 핵심 가치

| 가치        | 설명                                        | 측정 지표             |
| ----------- | ------------------------------------------- | --------------------- |
| 기록        | 일자별로 정리된 신뢰 가능한 사건 아카이브   | 일평균 등록 사건 수   |
| 추적        | 이슈의 상태/업데이트 로그 기반 최신화       | 이슈당 평균 트리거 수 |
| 참여        | 커뮤니티를 통한 맥락 공유 및 관심 이슈 확산 | DAU, 게시글/댓글 수   |
| 한눈에 보기 | 메인에서 속보/트렌드/지표를 빠르게 파악     | 메인 페이지 체류 시간 |

### 1.4 성공 지표

| 지표                     | 목표     | 측정 방법                            |
| ------------------------ | -------- | ------------------------------------ |
| 일간 활성 사용자 (DAU)   | 1,000+   | 서버 로그 기반 고유 사용자 수        |
| 이슈 추적 등록률         | 30%+     | 로그인 사용자 중 1개 이상 추적 비율  |
| 커뮤니티 게시글/댓글 수  | 50건/일+ | posts, comments 테이블 일별 집계     |
| 메인 페이지 이탈률       | < 40%    | 메인 진입 후 다른 페이지 이동 비율   |

---

## 2. 페르소나

### 2.1 뉴스 소비자 — 김민수 (29세, 직장인)

- **맥락**: 출퇴근 시간에 시사 이슈를 훑어보지만, 여러 뉴스 앱을 돌아다니며 파편화된 정보를 소비한다
- **니즈**: 흩어진 뉴스를 한 곳에서 시간순으로 파악하고 싶다
- **서비스 숙련도**: 중급
- **핵심 동기**: "오늘 무슨 일이 있었는지 5분 안에 파악하고 싶다"

### 2.2 이슈 추적자 — 이수진 (34세, 프리랜서 기자)

- **맥락**: 특정 사건의 경과를 지속적으로 추적하며, 후속 보도 시점을 놓치지 않아야 한다
- **니즈**: 관심 이슈에 새로운 업데이트가 생기면 즉시 알고 싶다
- **서비스 숙련도**: 고급
- **핵심 동기**: "관심 이슈의 상태 변화를 놓치지 않고 싶다"

### 2.3 어드민 — 운영자 (1인 개발자 겸 운영)

- **맥락**: 사건/이슈 데이터 품질을 관리하고, 커뮤니티 모더레이션을 수행하며, 데이터 파이프라인을 모니터링한다
- **니즈**: 최소한의 노력으로 서비스 전체를 관리할 수 있는 어드민 도구
- **서비스 숙련도**: 고급
- **핵심 동기**: "자동화된 파이프라인으로 운영 부담을 최소화하고 싶다"

---

## 3. 메인 시나리오

### 시나리오 A: 오늘의 사건 파악 (뉴스 소비자)

1. 민수가 출근길에 트렌드 코리아 메인 페이지에 접속한다
2. 속보 섹션에서 최신 사건/트리거 10건을 훑어본다
3. 관심 있는 사건을 탭하여 상세 모달에서 출처와 요약을 확인한다
4. 타임라인 페이지로 이동하여 어제부터 오늘까지의 사건을 시간순으로 탐색한다

### 시나리오 B: 이슈 추적 (이슈 추적자)

1. 수진이 이슈 추적 페이지에서 "진행중" 필터로 활성 이슈를 확인한다
2. 관심 이슈의 상세 페이지에서 업데이트 타임라인을 확인한다
3. "추적" 버튼을 눌러 해당 이슈를 관심 목록에 등록한다
4. 내 추적 페이지에서 NEW 뱃지가 달린 업데이트를 확인한다

### 시나리오 C: 어드민 운영

1. 운영자가 어드민 대시보드에서 주요 지표(오늘 등록 사건, 활성 사용자, 신고)를 확인한다
2. 데이터 파이프라인 모니터링에서 마지막 뉴스 수집 결과와 에러 로그를 확인한다
3. 신고된 게시글을 검토하여 삭제 또는 유지 결정을 내린다

---

## 4. 도메인 모델

### 4.1 핵심 엔티티 (DB 기준)

```
Event (사건) — events
├── id: String(36) PK
├── occurred_at: DateTime(tz)        # 발생 일시
├── title: String(50)                # 제목
├── summary: Text                    # 2~3줄 요약
├── importance: Enum(low|medium|high)
├── verification_status: Enum(verified|unverified)
├── source_count: Integer            # 비정규화 카운트
├── created_at: DateTime(tz)
└── updated_at: DateTime(tz)

Issue (이슈) — issues
├── id: String(36) PK
├── title: String(50)
├── description: Text
├── status: Enum(ongoing|closed|reignited|unverified)
├── tracker_count: Integer           # 비정규화 카운트
├── latest_trigger_at: DateTime(tz)? # 최근 트리거 시점
├── created_at: DateTime(tz)
└── updated_at: DateTime(tz)

Trigger (트리거) — triggers
├── id: String(36) PK
├── issue_id: FK → issues.id
├── occurred_at: DateTime(tz)
├── summary: Text
├── type: Enum(article|ruling|announcement|correction|status_change)
├── created_at: DateTime(tz)
└── updated_at: DateTime(tz)

User (사용자) — users
├── id: String(36) PK
├── nickname: String(50) UNIQUE
├── email: String(255) UNIQUE
├── password_hash: String(255)
├── profile_image: String(500)?
├── role: Enum(guest|member|admin)
├── is_active: Boolean
├── withdrawn_at: DateTime(tz)?
├── created_at: DateTime(tz)
└── updated_at: DateTime(tz)

UserSocialAccount — user_social_accounts
├── id: String(36) PK
├── user_id: FK → users.id
├── provider: String(20)             # kakao|naver|google
├── provider_user_id: String(100)
├── email: String(255)?
└── created_at: DateTime(tz)

Post (게시글) — posts
├── id: String(36) PK
├── author_id: FK → users.id
├── title: String(100)
├── content: Text (마크다운)
├── is_anonymous: Boolean
├── like_count: Integer
├── dislike_count: Integer
├── comment_count: Integer
├── created_at: DateTime(tz)
└── updated_at: DateTime(tz)

Comment (댓글) — comments
├── id: String(36) PK
├── post_id: FK → posts.id
├── parent_id: FK → comments.id?     # 대댓글
├── author_id: FK → users.id
├── content: Text
├── like_count: Integer
├── created_at: DateTime(tz)
└── updated_at: DateTime(tz)

Tag (태그) — tags
├── id: String(36) PK
├── name: String(50)
├── type: Enum(category|region)
├── slug: String(80) UNIQUE
└── updated_at: DateTime(tz)

Source (출처) — sources
├── id: String(36) PK
├── entity_type: Enum(event|issue|trigger)
├── entity_id: String(36)            # 다형성 FK
├── url: String(500)
├── title: String(255)
├── publisher: String(100)
└── published_at: DateTime(tz)

SearchRanking — search_rankings
├── id: String(36) PK
├── keyword: String(100)
├── rank: Integer
├── score: Integer
└── calculated_at: DateTime(tz)

SearchHistory — search_histories
├── id: String(36) PK
├── user_id: String(36)
├── keyword: String(100)
└── created_at: DateTime(tz)

RefreshToken — refresh_tokens
├── id: String(36) PK
├── user_id: FK → users.id
├── token_hash: String(64) UNIQUE
├── jti: String(36) UNIQUE
├── expires_at: DateTime(tz)
├── revoked_at: DateTime(tz)?
└── created_at: DateTime(tz)
```

### 4.2 연관 테이블 (Many-to-Many)

| 테이블               | 관계                   | 추가 컬럼              |
| -------------------- | ---------------------- | ---------------------- |
| `event_tags`         | Event ↔ Tag            | —                      |
| `issue_tags`         | Issue ↔ Tag            | —                      |
| `issue_events`       | Issue ↔ Event          | —                      |
| `post_tags`          | Post ↔ Tag             | —                      |
| `user_saved_events`  | User ↔ Event (저장)    | `saved_at: DateTime`   |
| `user_tracked_issues`| User ↔ Issue (추적)    | `tracked_at: DateTime` |

### 4.3 투표/좋아요

| 테이블          | 대상    | 필드                                       |
| --------------- | ------- | ------------------------------------------ |
| `post_votes`    | Post    | `vote_type: String(10)` (like/dislike)     |
| `comment_likes` | Comment | 좋아요만 (UniqueConstraint: comment+user)  |

### 4.4 데이터 파이프라인 테이블

| 테이블                   | 용도                                     |
| ------------------------ | ---------------------------------------- |
| `raw_articles`           | 크롤링된 원본 뉴스 기사                  |
| `news_summary_batches`   | 뉴스 요약 배치 메타데이터                |
| `news_keyword_summaries` | OpenAI로 요약된 키워드별 뉴스            |
| `news_summary_tags`      | 뉴스 요약 태그 연결                      |
| `crawled_keywords`       | 수집된 트렌드 키워드                     |
| `keyword_intersections`  | 키워드 교차 분석 결과                    |
| `issue_keyword_states`   | 이슈-키워드 연결 상태 (active/cooldown/closed) |
| `news_channels`          | 뉴스 채널 메타데이터 (broadcast/newspaper/online) |
| `naver_news_articles`    | 네이버 뉴스 크롤링 데이터                |
| `event_updates`          | 사건 업데이트 분류 결과 (NEW/MINOR/MAJOR/DUP) |
| `live_feed_items`        | 실시간 피드 아이템                       |
| `product_info`           | 상품 메타데이터 (Phase 2)                |
| `product_prices`         | 상품 가격 이력 (Phase 2)                 |
| `job_runs`               | 스케줄러 잡 실행 이력                    |

### 4.5 상태 정의

**이슈 상태 (IssueStatus)**

| 상태     | 코드         | 설명                |
| -------- | ------------ | ------------------- |
| 진행중   | `ongoing`    | 현재 진행 중인 이슈 |
| 종결     | `closed`     | 마무리된 이슈       |
| 재점화   | `reignited`  | 종결 후 재부상      |
| 확인필요 | `unverified` | 사실 확인 필요      |

**사건 검증 상태 (VerificationStatus)**

| 상태     | 코드         | 설명                  |
| -------- | ------------ | --------------------- |
| 확인됨   | `verified`   | 출처로 사실 확인 완료 |
| 확인필요 | `unverified` | 확인 대기             |

**사건 중요도 (Importance)**

| 등급 | 코드     |
| ---- | -------- |
| 높음 | `high`   |
| 중간 | `medium` |
| 낮음 | `low`    |

**트리거 유형 (TriggerType)**

| 유형     | 코드            |
| -------- | --------------- |
| 기사     | `article`       |
| 판결     | `ruling`        |
| 공식발표 | `announcement`  |
| 정정     | `correction`    |
| 상태변경 | `status_change` |

---

## 5. 기술 스택 (통합)

### 5.1 프론트엔드

| 레이어         | 기술                                    |
| -------------- | --------------------------------------- |
| 프레임워크     | Next.js 16 (App Router)                 |
| 언어           | TypeScript 5.9 (strict mode)            |
| 스타일링       | Tailwind CSS 4                          |
| UI 라이브러리  | shadcn/ui |
| 데이터 페칭    | React Query (TanStack Query)            |
| 폼 검증        | Zod                                     |
| 린팅/포맷팅    | ESLint 9 (flat config) + Prettier       |
| Git Hooks      | lefthook                                |
| 패키지 매니저  | pnpm                                    |
| Node.js        | v24.13.0                                |

### 5.2 백엔드

| 레이어         | 기술                                    |
| -------------- | --------------------------------------- |
| 프레임워크     | FastAPI (Python 3.11+)                  |
| ORM            | SQLAlchemy 2.0 (sync)                   |
| 검증           | Pydantic V2                             |
| DB             | PostgreSQL 16                           |
| 마이그레이션   | Alembic                                 |
| 스케줄러       | APScheduler (BlockingScheduler)         |
| AI/LLM         | OpenAI API (뉴스 요약)                  |
| 린터/포매터    | ruff (line-length=100)                  |
| 패키지 매니저  | uv                                      |

### 5.3 어드민

| 레이어         | 기술                                    |
| -------------- | --------------------------------------- |
| 프레임워크     | Next.js (App Router, Fullstack)         |
| 언어           | TypeScript 5.9 (strict mode)            |
| ORM            | Prisma (introspection)                  |
| 스타일링       | Tailwind CSS 4                          |
| UI 라이브러리  | shadcn/ui                               |
| 인증           | 공유 JWT 검증 (jose)                    |
| 패키지 매니저  | pnpm                                    |

### 5.4 인프라 (계획)

| 항목           | 기술/서비스                             |
| -------------- | --------------------------------------- |
| 컨테이너       | Docker + docker-compose                 |
| API 서버       | uvicorn (FastAPI)                       |
| 스케줄러 워커  | 별도 프로세스 (`trend-korea-worker`)    |
| DB             | PostgreSQL 16                           |
| 배포           | 추후 결정 (VPS / Cloud Run 등)          |
| CI/CD          | 추후 결정                               |

---

## 6. 아키텍처

### 6.1 시스템 전체 구조

```
┌─────────────────────────────────────────────────────────┐
│                    사용자 (브라우저)                      │
└────────────┬────────────────────────────────────────────┘
             │
     ┌───────┴───────┐
     ▼               ▼
┌──────────────┐  ┌──────────────────────────────────────┐
│ trend-korea- │  │ trend-korea-app (Next.js 16)          │
│ admin        │  │  ┌────────────────────────────────┐   │
│ (Next.js     │  │  │  app/ → features/ → shared/    │   │
│  Fullstack)  │  │  │  8 features: auth, home,       │   │
│              │  │  │  timeline, issues, community,  │   │
│ Prisma ORM   │  │  │  tracking, search, mypage      │   │
│ shadcn/ui    │  │  └────────────────────────────────┘   │
│ JWT 검증     │  │  React Query ── Zod ── shadcn/ui      │
│ (jose)       │  └──────────────────┬───────────────────┘
│              │                     │ REST API (JSON)
│              │  ┌──────────────────▼───────────────────┐
│              │  │ trend-korea-api (FastAPI :8000)       │
│              │  │  ┌────────────────────────────────┐   │
│              │  │  │  api/v1/ → schemas/ → crud/    │   │
│              │  │  │  → sql/ → models/              │   │
│              │  │  │  13 라우터                       │   │
│              │  │  └────────────────────────────────┘   │
│              │  │  JWT 발급 ── Pydantic V2 ──           │
│              │  │  SQLAlchemy 2.0                       │
│              │  └──────────────────┬───────────────────┘
│              │                     │
└──────┬───────┘       ┌─────────────┼──────────────┐
       │               ▼             ▼              ▼
       │        ┌────────────┐ ┌────────────┐ ┌────────────┐
       │        │ PostgreSQL │ │  OpenAI    │ │ 외부 뉴스  │
       └───────>│    16      │ │  API       │ │ 사이트     │
  Prisma 직접   └────────────┘ └────────────┘ └────────────┘
  접근                 ▲
                       │
┌──────────────────────┴──────────────────────────────────┐
│            스케줄러 워커 (trend-korea-worker)             │
│  APScheduler (BlockingScheduler)                         │
│  ┌─────────────────────────────────────────────┐        │
│  │  Jobs:                                       │        │
│  │  - news_collect (15분): 뉴스 수집 파이프라인 │        │
│  │  - keyword_state_cleanup (1시간)             │        │
│  │  - search_rankings (1시간): 검색 순위 갱신   │        │
│  │  - community_hot_score (10분)                │        │
│  │  - issue_status_reconcile (30분)             │        │
│  │  - cleanup_refresh_tokens (매일 03:00)       │        │
│  └─────────────────────────────────────────────┘        │
└─────────────────────────────────────────────────────────┘

* trend-korea-admin은 PostgreSQL에 Prisma ORM으로 직접 접근한다.
* JWT 토큰은 trend-korea-api가 발급하고, admin은 jose 라이브러리로 검증만 수행한다.
```

### 6.2 프론트엔드 아키텍처

**Features + Shared 패턴**: `app -> features -> shared` (역방향 금지)

```
src/
├── app/                          # 라우팅 & features 조립만
│   ├── (auth)/                   # 인증 라우트 그룹
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   ├── (main)/                   # 메인 라우트 그룹
│   │   ├── page.tsx              # 홈 (/)
│   │   ├── timeline/
│   │   ├── issues/
│   │   ├── community/
│   │   ├── search/
│   │   ├── tracking/
│   │   └── mypage/
│   └── layout.tsx
├── features/                     # 도메인 기능
│   ├── auth/
│   ├── home/
│   ├── timeline/
│   ├── issues/
│   ├── community/
│   ├── search/
│   ├── tracking/
│   └── mypage/
└── shared/                       # 도메인 중립 공유
    ├── layouts/
    ├── lib/
    ├── ui/
    ├── utils/
    ├── types/
    └── styles/
```

### 6.3 백엔드 레이어 구조

```
api/v1/{domain}.py      # 라우터: HTTP 요청/응답, 인증 체크
       ↓
schemas/{domain}.py      # 스키마: Pydantic V2 요청/응답 모델
       ↓
crud/{domain}.py         # 비즈니스 로직: 유효성 검증, 조합
       ↓
sql/{domain}.py          # 데이터 액세스: SQLAlchemy 쿼리
       ↓
models/{domain}.py       # 모델: SQLAlchemy ORM 모델
```

---

## 7. 기능 요구사항 (사용자 서비스)

### REQ-1: 메인 페이지 (홈)

#### Feature: 속보 피드 (FR-M1) — P0

- **사용자 스토리**: 사용자로서 메인 페이지에서 최신 사건/트리거를 빠르게 확인할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M1-1 | 사용자가 최신 사건/트리거 10건을 시각, 제목, 1줄 요약으로 본다 | `GET /api/v1/home/breaking-news` |
| S-M1-2 | 항목 클릭 시 사건 상세 모달 또는 이슈 상세 페이지로 이동한다 | — |

#### Feature: 타임라인 미니맵 (FR-M2) — P0

- **사용자 스토리**: 사용자로서 최근 7일간 사건 밀도를 시각적으로 확인하고 특정 날짜로 이동할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M2-1 | 사용자가 최근 7일 사건 수를 날짜별 막대/점으로 본다 | `GET /api/v1/home/timeline-minimap` |
| S-M2-2 | 날짜 클릭 시 `/timeline/{date}` 페이지로 이동한다 | — |

#### Feature: 커뮤니티 핫 게시물 (FR-M3) — P0

- **사용자 스토리**: 사용자로서 최근 24시간 인기 게시물 5건을 확인할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M3-1 | 사용자가 인기 게시물 5건을 제목, 추천 수, 댓글 수와 함께 본다 | `GET /api/v1/home/hot-posts` |

#### Feature: 실시간 검색순위 (FR-M4) — P0

- **사용자 스토리**: 사용자로서 현재 인기 검색 키워드 1~10위를 확인할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M4-1 | 사용자가 1~10위 키워드와 순위 변동을 본다 (1시간 갱신) | `GET /api/v1/home/search-rankings` |
| S-M4-2 | 키워드 클릭 시 `/search?q={keyword}`로 이동한다 | — |

#### Feature: 대한민국 트렌드 (FR-M5) — P0

- **사용자 스토리**: 사용자로서 24h/7d/30d 급상승 이슈/태그를 확인할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M5-1 | 사용자가 기간별(24h/7d/30d) 급상승 이슈/태그 최대 5개를 본다 | `GET /api/v1/home/trending` |

#### Feature: 주요 뉴스 (FR-M6) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M6-1 | 사용자가 주요 뉴스 요약을 확인할 수 있다 | `GET /api/v1/home/featured-news` |

#### Feature: 커뮤니티 미디어 (FR-M7) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-M7-1 | 사용자가 커뮤니티 미디어 콘텐츠를 확인할 수 있다 | `GET /api/v1/home/community-media` |

---

### REQ-2: 타임라인

#### Feature: 날짜 선택기 (FR-T1) — P0

- **사용자 스토리**: 사용자로서 달력에서 특정 날짜를 선택하여 해당 일의 사건을 볼 수 있다

| Spec ID | 명세 | 비고 |
|---------|------|------|
| S-T1-1 | 사용자가 달력 모달을 열어 날짜를 선택한다 | 사건 존재 일자에 점 표시 |
| S-T1-2 | "오늘" 버튼으로 현재 날짜로 즉시 이동한다 | — |

#### Feature: 필터/정렬 (FR-T2, FR-T3) — P0

- **사용자 스토리**: 사용자로서 분야/지역/중요도로 사건을 필터링하고, 시간순 또는 중요도순으로 정렬할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-T2-1 | 사용자가 분야(정치/사회/사법 등), 지역, 중요도 필터를 적용한다 | `GET /api/v1/events?tag_ids=&importance=` |
| S-T2-2 | 사용자가 시간순(기본) 또는 중요도순으로 정렬한다 | `GET /api/v1/events?sort=occurred_at|importance` |

#### Feature: 사건 카드 (FR-T4) — P0

- **사용자 스토리**: 사용자로서 사건의 핵심 정보(시각, 제목, 요약, 태그, 출처 수, 상태)를 카드 형태로 확인할 수 있다

| Spec ID | 명세 | 비고 |
|---------|------|------|
| S-T4-1 | 카드에 발생 시각, 제목, 요약, 태그, 출처 수, 검증 상태 뱃지를 표시한다 | — |
| S-T4-2 | 사용자가 저장/공유 버튼으로 사건을 북마크하거나 링크를 복사한다 | 저장: `POST /api/v1/events/{id}/save` |

#### Feature: 무한 스크롤 (FR-T5) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-T5-1 | 사용자가 스크롤하면 일주일 단위로 과거 사건이 추가 로딩된다 | `GET /api/v1/events?cursor=&limit=` |

#### Feature: 사건 상세 모달 (FR-T6) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-T6-1 | 사용자가 사건을 클릭하면 전체 내용, 출처 링크, 관련 이슈를 모달에서 확인한다 | `GET /api/v1/events/{id}` |
| S-T6-2 | 관련 이슈 클릭 시 이슈 상세 페이지로 이동한다 | — |

**Userflow (타임라인)**:
1. 사용자가 `/timeline` 페이지에 진입한다
2. 오늘 날짜의 사건 목록이 시간순으로 표시된다
3. 사용자가 필터/정렬을 조정하거나 달력에서 날짜를 선택한다
4. 사건 카드를 클릭하면 상세 모달이 열린다
5. 아래로 스크롤하면 이전 주 사건이 로딩된다

---

### REQ-3: 이슈 추적

#### Feature: 이슈 목록/필터/정렬 (FR-I1, FR-I2) — P0

- **사용자 스토리**: 사용자로서 이슈를 상태/분야/기간으로 필터링하고 업데이트순/추적자순으로 정렬할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-I1-1 | 사용자가 상태(ongoing/closed/reignited/unverified), 분야, 기간으로 필터링한다 | `GET /api/v1/issues?status=&tag_ids=` |
| S-I1-2 | 사용자가 최신 업데이트순(기본)/추적자순/중요도순으로 정렬한다 | `GET /api/v1/issues?sort=latest_trigger_at|tracker_count` |

#### Feature: 이슈 카드 (FR-I3) — P0

| Spec ID | 명세 | 비고 |
|---------|------|------|
| S-I3-1 | 카드에 상태 아이콘, 제목, 최근 업데이트 요약, 태그, 추적자 수를 표시한다 | — |
| S-I3-2 | "추적" / "상세보기" 버튼을 표시한다 | — |

#### Feature: 이슈 상세 (FR-I4) — P0

- **사용자 스토리**: 사용자로서 이슈의 전체 배경, 업데이트 타임라인, 관련 사건, 출처를 확인할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-I4-1 | 사용자가 이슈 개요(제목, 설명, 상태, 태그)를 확인한다 | `GET /api/v1/issues/{id}` |
| S-I4-2 | 사용자가 트리거 타임라인을 시간순으로 확인한다 | `GET /api/v1/issues/{id}/triggers` |
| S-I4-3 | 사용자가 관련 사건 목록을 확인한다 | `GET /api/v1/issues/{id}/timeline` |
| S-I4-4 | 사용자가 출처 목록을 확인하고 원문 링크로 이동한다 | `GET /api/v1/sources?entity_type=issue&entity_id={id}` |

#### Feature: 추적 등록/해제 (FR-I5) — P0

- **사용자 스토리**: 로그인한 사용자로서 관심 이슈를 추적 목록에 추가/제거할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-I5-1 | 사용자가 "추적" 버튼을 눌러 이슈를 추적 등록한다 (로그인 필요) | `POST /api/v1/issues/{id}/track` |
| S-I5-2 | 사용자가 "추적 해제" 버튼을 눌러 추적을 취소한다 | `DELETE /api/v1/issues/{id}/track` |
| S-I5-3 | 추적 등록/해제 시 `tracker_count`가 실시간 반영된다 | — |

---

### REQ-4: 커뮤니티

#### Feature: 탭/정렬 (FR-C1, FR-C5) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-C1-1 | 사용자가 최신/인기(24h 추천)/핫(추천+댓글 복합) 탭으로 전환한다 | `GET /api/v1/posts?tab=latest|popular|hot` |
| S-C1-2 | 사용자가 최신순/추천순/댓글 많은 순으로 정렬한다 | `GET /api/v1/posts?sort=created_at|like_count|comment_count` |

#### Feature: 글쓰기 (FR-C3) — P0

- **사용자 스토리**: 로그인한 사용자로서 제목, 본문, 태그를 입력하여 게시글을 작성할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-C3-1 | 사용자가 제목(필수, 100자), 본문(마크다운 에디터), 태그(최대 3개), 익명 여부를 입력한다 | `POST /api/v1/posts` |
| S-C3-2 | 사용자가 작성한 글을 수정한다 | `PATCH /api/v1/posts/{id}` |
| S-C3-3 | 사용자가 작성한 글을 삭제한다 | `DELETE /api/v1/posts/{id}` |

**엣지 케이스**:
- 비로그인 상태에서 글쓰기 시도 → 로그인 페이지로 리다이렉트
- 다른 사용자의 글 수정/삭제 시도 → 403 Forbidden

#### Feature: 게시글 상세/댓글 (FR-C4) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-C4-1 | 사용자가 게시글 전체 내용을 마크다운 렌더링으로 확인한다 | `GET /api/v1/posts/{id}` |
| S-C4-2 | 사용자가 댓글/대댓글을 트리 구조로 확인한다 | `GET /api/v1/posts/{id}/comments` |
| S-C4-3 | 사용자가 댓글을 작성한다 (대댓글은 `parent_id` 지정) | `POST /api/v1/posts/{id}/comments` |
| S-C4-4 | 사용자가 게시글에 추천/비추천을 한다 (중복 투표 불가) | `POST /api/v1/posts/{id}/like` |
| S-C4-5 | 사용자가 댓글을 수정한다 | `PATCH /api/v1/comments/{id}` |
| S-C4-6 | 사용자가 댓글을 삭제한다 | `DELETE /api/v1/comments/{id}` |
| S-C4-7 | 사용자가 댓글에 좋아요를 누른다/취소한다 | `POST/DELETE /api/v1/comments/{id}/like` |

---

### REQ-5: 검색

#### Feature: 통합 검색 (FR-S1~S5) — P0

- **사용자 스토리**: 사용자로서 키워드로 사건/이슈/게시글을 통합 검색하고 탭별로 결과를 확인할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-S1-1 | 사용자가 검색어를 입력하면 자동완성 제안을 받는다 | 프론트엔드 디바운스 → `GET /api/v1/search` |
| S-S1-2 | 사용자가 최근 검색어 10개를 확인하고 삭제한다 | 로컬스토리지 또는 `search_histories` |
| S-S2-1 | 사용자가 기간/분야/지역/중요도 고급 필터를 적용한다 | `GET /api/v1/search?q=&period=&tag_ids=` |
| S-S3-1 | 사용자가 전체/사건/이슈/커뮤니티 탭으로 결과를 분류하여 본다 | `GET /api/v1/search/events`, `/issues`, `/posts` |
| S-S4-1 | 검색 결과에 제목 하이라이트, 날짜, 요약, 태그를 표시한다 | — |
| S-S5-1 | 사용자가 관련도순(기본)/최신순/인기순으로 정렬한다 | `GET /api/v1/search?sort=relevance|date|popularity` |

---

### REQ-6: 내 추적

#### Feature: 추적/저장 모아보기 (FR-M1~M4) — P0

- **사용자 스토리**: 로그인한 사용자로서 추적 중인 이슈와 저장한 사건을 한 곳에서 관리할 수 있다

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-TR1-1 | 사용자가 "추적 중인 이슈" 탭에서 추적 이슈 목록을 확인한다 | `GET /api/v1/users/me/tracked-issues` |
| S-TR1-2 | 사용자가 "저장한 사건" 탭에서 저장 사건 목록을 확인한다 | `GET /api/v1/users/me/saved-events` |
| S-TR1-3 | 새 업데이트가 있는 항목에 NEW 뱃지를 표시한다 | — |
| S-TR1-4 | 최신 업데이트순/저장 날짜순/이름순으로 정렬한다 | 쿼리 파라미터 `sort` |
| S-TR1-5 | 추적 중인 항목이 없으면 빈 상태 안내를 표시한다 | — |

---

### REQ-7: MY 페이지

#### Feature: 프로필/계정 관리 (FR-MY1~MY4) — P0

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-MY1-1 | 사용자가 프로필(이미지, 닉네임, 이메일, 가입일)을 확인한다 | `GET /api/v1/users/me` |
| S-MY2-1 | 사용자가 닉네임/프로필 이미지를 수정한다 | `PATCH /api/v1/users/me` |
| S-MY2-2 | 사용자가 비밀번호를 변경한다 | `POST /api/v1/users/me/change-password` |
| S-MY2-3 | 사용자가 SNS 계정을 연동/해제한다 | `POST /api/v1/users/me/social-connect`, `DELETE /api/v1/users/me/social-disconnect` |
| S-MY3-1 | 사용자가 활동 내역(작성 게시글/댓글 수)을 확인한다 | `GET /api/v1/users/me/activity` |
| S-MY4-1 | 사용자가 로그아웃한다 | `POST /api/v1/auth/logout` |
| S-MY4-2 | 사용자가 회원탈퇴한다 (soft delete, `withdrawn_at` 기록) | `DELETE /api/v1/auth/withdraw` |

---

### REQ-8: 실시간 피드

#### Feature: 라이브 피드 (FR-F1) — P1

| Spec ID | 명세 | API 엔드포인트 |
|---------|------|---------------|
| S-F1-1 | 사용자가 실시간 뉴스 피드를 타입별(breaking/major/all)로 확인한다 | `GET /api/v1/feed/live` |

---

## 8. 기능 요구사항 (어드민)

> 어드민 기능은 독립 프로젝트 `trend-korea-admin`으로 분리되었습니다.
> 상세 명세는 [`trend-korea-admin/docs/PRD.md`](../trend-korea-admin/docs/PRD.md)를 참조하세요.

### 요약

| Feature | 설명 | 우선순위 |
|---------|------|---------|
| FR-AD1 | 대시보드 — 주요 지표 및 최근 활동 | P1 |
| FR-AD2 | 사건/이슈/트리거 CRUD | P0 |
| FR-AD3 | 태그/출처 관리 | P0 |
| FR-AD4 | 사용자 관리 | P1 |
| FR-AD5 | 커뮤니티 모더레이션 | P1 |
| FR-AD6 | 데이터 파이프라인 모니터링 | P1 |

---

## 9. 데이터 수집 파이프라인 요구사항

### 9.1 뉴스 수집 파이프라인 (news_collect)

**실행 주기**: 15분 간격 (설정 가능: `schedule_news_collect_minutes`)

**파이프라인 단계**:

```
키워드 수집 → 뉴스 크롤링 → 분류/중복 제거 → 요약 → 피드 저장
```

| 단계 | 처리 내용 | 관련 모듈 |
|------|----------|----------|
| 1. 키워드 수집 | 트렌드 키워드를 수집하고 교차 분석한다 | `utils/keyword_crawler/` |
| 2. 뉴스 크롤링 | 키워드 기반으로 뉴스 기사를 수집한다 (네이버 뉴스 API 옵션) | `utils/news_crawler/`, `utils/naver_news_crawler/` |
| 3. 분류/중복 제거 | 수집된 기사를 NEW/MINOR_UPDATE/MAJOR_UPDATE/DUP로 분류한다 | `utils/pipeline/update_classifier.py` |
| 4. 요약 | OpenAI API로 기사를 요약한다 | `utils/news_summarizer/` |
| 5. 피드 저장 | 결과를 피드 아이템으로 저장한다 | `utils/pipeline/feed_builder.py` |

**설정 파라미터**:

| 파라미터 | 기본값 | 설명 |
|---------|--------|------|
| `top_n` | 30 | 수집할 상위 키워드 수 |
| `max_keywords` | 5 | 크롤링에 사용할 최대 키워드 수 |
| `limit` | 3 | 키워드당 최대 기사 수 |
| `keyword_strategy` | `intersection` | 키워드 선택 전략 |
| `enable_classification` | `true` | 분류 활성화 여부 |

### 9.2 키워드 상태 정리 (keyword_state_cleanup)

**실행 주기**: 1시간 간격 (설정 가능: `schedule_keyword_cleanup_minutes`)

| 전환 | 조건 |
|------|------|
| ACTIVE → COOLDOWN | `last_seen_at`이 48시간 경과 |
| COOLDOWN → CLOSED | `last_seen_at`이 120시간(48+72) 경과 |

### 9.3 기타 스케줄러 잡

| 잡 이름 | 주기 | 설명 |
|---------|------|------|
| `issue_status_reconcile` | 30분 | 이슈 상태 정합성 검증/자동 보정 |
| `search_rankings` | 1시간 | 검색 순위 재계산 |
| `community_hot_score` | 10분 | 커뮤니티 인기 점수 재계산 |
| `cleanup_refresh_tokens` | 매일 03:00 | 만료된 리프레시 토큰 정리 |

### 9.4 잡 실행 추적

모든 잡은 `job_runs` 테이블에 실행 이력을 기록한다:

| 필드 | 설명 |
|------|------|
| `job_name` | 잡 이름 |
| `status` | `success` / `failed` |
| `detail` | 실행 결과 상세 (기사 수, 에러 메시지 등) |
| `started_at` | 시작 시각 |
| `finished_at` | 종료 시각 |

---

## 10. API 설계

### 10.1 기본 원칙

- **프리픽스**: `/api/v1/`
- **응답 형식**: JSON
- **인증**: Bearer JWT (Access Token)
- **에러 응답**: `{ "detail": "에러 메시지" }` + 적절한 HTTP 상태 코드
- **페이지네이션**: `page` + `limit` 또는 커서 기반

### 10.2 엔드포인트 매핑

> **참고**: `admin` 권한이 필요한 CUD(Create/Update/Delete) 엔드포인트는 `trend-korea-admin`의 Next.js Route Handlers로 이관되었습니다. 어드민 앱은 Prisma ORM으로 PostgreSQL에 직접 접근하므로, 아래 API 엔드포인트 중 "(→ trend-korea-admin)" 표시된 항목은 사용자 앱에서는 호출하지 않습니다.

#### 인증 (auth)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `POST` | `/auth/register` | 회원가입 | 불필요 |
| `POST` | `/auth/login` | 로그인 | 불필요 |
| `POST` | `/auth/refresh` | 토큰 갱신 | Refresh Token |
| `POST` | `/auth/logout` | 로그아웃 | 필요 |
| `GET` | `/auth/social/providers` | SNS 로그인 제공자 목록 | 불필요 |
| `POST` | `/auth/social-login` | SNS 로그인 | 불필요 |
| `DELETE` | `/auth/withdraw` | 회원탈퇴 | 필요 |

#### 사용자 (users / me)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/users/me` | 내 정보 조회 | 필요 |
| `PATCH` | `/users/me` | 내 정보 수정 | 필요 |
| `POST` | `/users/me/change-password` | 비밀번호 변경 | 필요 |
| `POST` | `/users/me/social-connect` | SNS 연동 | 필요 |
| `DELETE` | `/users/me/social-disconnect` | SNS 연동 해제 | 필요 |
| `GET` | `/users/me/activity` | 내 활동 내역 | 필요 |
| `GET` | `/users/{id}` | 사용자 공개 프로필 | 불필요 |

#### 사건 (events)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/events` | 사건 목록 (필터/정렬/페이지네이션) | 불필요 |
| `GET` | `/events/{id}` | 사건 상세 | 불필요 |
| `POST` | `/events/{id}/save` | 사건 저장 | member |
| `DELETE` | `/events/{id}/save` | 사건 저장 해제 | member |
| `POST` | `/events` | 사건 생성 | admin (→ trend-korea-admin) |
| `PATCH` | `/events/{id}` | 사건 수정 | admin (→ trend-korea-admin) |
| `DELETE` | `/events/{id}` | 사건 삭제 | admin (→ trend-korea-admin) |

#### 이슈 (issues)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/issues` | 이슈 목록 | 불필요 |
| `GET` | `/issues/{id}` | 이슈 상세 | 불필요 |
| `POST` | `/issues` | 이슈 생성 | admin (→ trend-korea-admin) |
| `PATCH` | `/issues/{id}` | 이슈 수정 | admin (→ trend-korea-admin) |
| `DELETE` | `/issues/{id}` | 이슈 삭제 | admin (→ trend-korea-admin) |
| `GET` | `/issues/{id}/triggers` | 이슈 트리거 목록 | 불필요 |
| `POST` | `/issues/{id}/triggers` | 트리거 생성 | admin (→ trend-korea-admin) |
| `GET` | `/issues/{id}/timeline` | 이슈 타임라인 | 불필요 |
| `POST` | `/issues/{id}/track` | 이슈 추적 등록 | member |
| `DELETE` | `/issues/{id}/track` | 이슈 추적 해제 | member |

#### 트리거 (triggers)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `PATCH` | `/triggers/{id}` | 트리거 수정 | admin (→ trend-korea-admin) |
| `DELETE` | `/triggers/{id}` | 트리거 삭제 | admin (→ trend-korea-admin) |

#### 커뮤니티 (posts / comments)

> 커뮤니티 라우터는 prefix 없이 `/posts`, `/comments` 경로를 직접 사용한다.

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/posts` | 게시글 목록 | 불필요 |
| `POST` | `/posts` | 게시글 작성 | member |
| `GET` | `/posts/{id}` | 게시글 상세 | 불필요 |
| `PATCH` | `/posts/{id}` | 게시글 수정 | member (본인) |
| `DELETE` | `/posts/{id}` | 게시글 삭제 | member (본인) / admin (→ trend-korea-admin) |
| `GET` | `/posts/{id}/comments` | 댓글 목록 | 불필요 |
| `POST` | `/posts/{id}/comments` | 댓글 작성 | member |
| `POST` | `/posts/{id}/like` | 게시글 추천/비추천 | member |
| `PATCH` | `/comments/{id}` | 댓글 수정 | member (본인) |
| `DELETE` | `/comments/{id}` | 댓글 삭제 | member (본인) / admin (→ trend-korea-admin) |
| `POST` | `/comments/{id}/like` | 댓글 좋아요 | member |
| `DELETE` | `/comments/{id}/like` | 댓글 좋아요 취소 | member |

#### 검색 (search)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/search` | 통합 검색 | 불필요 |
| `GET` | `/search/events` | 사건 검색 | 불필요 |
| `GET` | `/search/issues` | 이슈 검색 | 불필요 |
| `GET` | `/search/posts` | 게시글 검색 | 불필요 |

#### 추적 (tracking)

> 추적 라우터는 `/users/me` prefix 아래에 위치한다.

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/users/me/tracked-issues` | 추적 중인 이슈 목록 | member |
| `GET` | `/users/me/saved-events` | 저장한 사건 목록 | member |

#### 홈 (home)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/home/breaking-news` | 속보 | 불필요 |
| `GET` | `/home/hot-posts` | 인기 게시물 | 불필요 |
| `GET` | `/home/search-rankings` | 검색 순위 | 불필요 |
| `GET` | `/home/trending` | 트렌드 | 불필요 |
| `GET` | `/home/timeline-minimap` | 타임라인 미니맵 | 불필요 |
| `GET` | `/home/featured-news` | 주요 뉴스 | 불필요 |
| `GET` | `/home/community-media` | 커뮤니티 미디어 | 불필요 |

#### 태그/출처 (tags, sources)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/tags` | 태그 목록 | 불필요 |
| `POST` | `/tags` | 태그 생성 | admin (→ trend-korea-admin) |
| `PATCH` | `/tags/{id}` | 태그 수정 | admin (→ trend-korea-admin) |
| `DELETE` | `/tags/{id}` | 태그 삭제 | admin (→ trend-korea-admin) |
| `GET` | `/sources` | 출처 목록 (entity_type/entity_id 필터) | 불필요 |
| `POST` | `/sources` | 출처 추가 | admin (→ trend-korea-admin) |
| `DELETE` | `/sources/{id}` | 출처 삭제 | admin (→ trend-korea-admin) |

#### 피드 (feed)

| 메서드 | 경로 | 설명 | 인증 |
|--------|------|------|------|
| `GET` | `/feed/live` | 실시간 피드 | 불필요 |

---

## 11. 인증/인가

### 11.1 인증 방식

| 항목 | 상세 |
|------|------|
| 가입 | 이메일 + 비밀번호 (bcrypt 해싱) |
| SNS 로그인 | 카카오 / 네이버 / 구글 OAuth |
| 토큰 | JWT Access Token (단기) + Refresh Token (장기) |
| 갱신 | Refresh Token으로 Access Token 재발급 |
| 저장 | Refresh Token은 `refresh_tokens` 테이블에 해시 저장 |
| 만료 정리 | 매일 03:00 `cleanup_refresh_tokens` 잡으로 만료 토큰 삭제 |

### 11.2 권한 매트릭스

| 기능 | guest | member | admin |
|------|-------|--------|-------|
| 사건/이슈/커뮤니티 조회, 검색 | O | O | O |
| 사건 저장 / 이슈 추적 | X | O | O |
| 게시글/댓글 작성 | X | O | O |
| 추천/비추천/좋아요 | X | O | O |
| 본인 게시글/댓글 수정/삭제 | X | O | O |
| 사건/이슈/트리거 CRUD | X | X | O |
| 태그/출처 CRUD | X | X | O |
| 타인 게시글/댓글 삭제 | X | X | O |
| 사용자 관리 | X | X | O |
| 파이프라인 모니터링 | X | X | O |

---

## 12. 화면 구성

### 12.1 정보 구조 (IA)

```
/                              # 메인 (홈)
/timeline                      # 타임라인
/timeline/:date                # 특정 날짜 타임라인
/issues                        # 이슈 추적 목록
/issues/:id                    # 이슈 상세
/community                     # 커뮤니티
/community/write               # 글쓰기
/community/:id                 # 게시글 상세
/community/:id/edit            # 게시글 수정
/search                        # 검색 (검색 결과: /search?q=)
/tracking                      # 내 추적
/mypage                        # MY 페이지
/mypage/edit                   # 회원 정보 수정
/login                         # 로그인
/register                      # 회원가입
/admin/*                       # 어드민 (→ trend-korea-admin 별도 앱)
```

### 12.2 페이지 간 연결 구조

```
메인 ──> 타임라인 ──> 사건 상세 모달 ──> 관련 이슈
  │                                        ↑
  ├──> 이슈 추적 ──> 이슈 상세 ──> 관련 사건
  │
  ├──> 커뮤니티 ──> 게시글 상세 ──> 관련 태그
  │
  ├──> 검색 ──> 사건/이슈/커뮤니티 결과
  │
  └──> 내 추적 ──> 저장한 사건/추적 이슈 상세
```

### 12.3 공통 필터 체계

| 필터   | 옵션                                               | 적용 페이지               |
| ------ | -------------------------------------------------- | ------------------------- |
| 기간   | 전체 / 1주일 / 1개월 / 3개월 / 직접 입력           | 타임라인, 이슈 추적, 검색 |
| 분야   | 정치, 사회, 사법, 재난, 경제, 노동, 외교안보, 문화 | 전체                      |
| 지역   | 전국, 서울, 경기, 인천, 강원, 충북, 충남...        | 타임라인, 검색            |
| 중요도 | 전체 / 높음 / 중간 / 낮음                          | 타임라인, 검색            |
| 상태   | 진행중 / 종결 / 재점화 / 확인필요                  | 이슈 추적 전용            |

---

## 13. 비기능 요구사항

### 13.1 성능

| 지표 | 목표 |
|------|------|
| FCP (First Contentful Paint) | < 1.5s |
| LCP (Largest Contentful Paint) | < 2.5s |
| API 응답 시간 (P95) | < 500ms |
| DB 쿼리 (P95) | < 100ms |

### 13.2 SEO

- 사건/이슈 상세 페이지는 SSR (Server-Side Rendering)
- 메타태그 + Open Graph 태그 동적 생성
- `sitemap.xml`, `robots.txt` 제공

### 13.3 보안

- 비밀번호: bcrypt 해싱 (cost factor 12)
- JWT: 짧은 Access Token 만료 시간 + Refresh Token 로테이션
- CORS: 프론트엔드 도메인만 허용
- Rate Limiting: 인증 엔드포인트에 적용 (추후)
- SQL Injection: SQLAlchemy 파라미터 바인딩으로 방지
- XSS: 마크다운 렌더링 시 sanitize 처리

### 13.4 접근성

- WCAG 2.1 AA 수준
- `aria-label`, `aria-invalid`, `aria-describedby` 적용
- 키보드 네비게이션 (Tab, Enter, Escape)
- 색상 대비 4.5:1 이상

### 13.5 반응형

- 모바일 퍼스트 설계
- 브레이크포인트: 모바일 (< 768px) / 태블릿 (768~1024px) / 데스크탑 (> 1024px)

### 13.6 모니터링

- 스케줄러 잡 실행 이력: `job_runs` 테이블
- 헬스체크: `GET /health/live` (Liveness), `GET /health/ready` (Readiness)

---

## 14. AI 에이전트 Boundaries

### Always (항상 지킬 것)

- 프론트엔드: shadcn/ui 컴포넌트 사용, HTML 요소 직접 사용 금지
- 프론트엔드: `features -> shared` 의존 방향 준수, feature 간 직접 import 금지
- 프론트엔드: named export 우선, `import { type Foo }` 인라인 형식
- 백엔드: `api → schemas → crud → sql → models` 레이어 순서 준수
- 백엔드: 새 모델 추가 시 `db/__init__.py`에 배럴 import 등록
- 백엔드: ruff 린트/포맷 통과 (line-length=100)
- 프론트엔드: ESLint + Prettier + lefthook 린트 통과
- 테스트: 새 기능은 반드시 테스트 코드 포함
- 도메인 용어: 코드 식별자는 영문 용어 사전 준수 (Event, Issue, Trigger 등)
- 타입 안전성: TypeScript strict mode (프론트), Pydantic V2 (백엔드)

### Ask First (먼저 확인할 것)

- DB 스키마 변경 (Alembic 마이그레이션)
- 새 외부 API/서비스 의존성 추가
- 인증/인가 로직 변경
- 스케줄러 잡 주기 변경
- OpenAI 모델/프롬프트 변경
- 새 라우트 그룹 추가
- 새 feature 모듈 추가

### Never (절대 하지 말 것)

- 프로덕션 DB 직접 접근 또는 데이터 삭제
- `.env`, 시크릿 키, API 키를 코드에 하드코딩
- 보안 미들웨어/인증 체크 우회
- `db/__init__.py` 배럴 import 누락
- feature 간 직접 import (반드시 `index.ts` 경유)
- 테스트 없이 비즈니스 로직 변경

### Commands

```bash
# 프론트엔드
pnpm dev                    # 개발 서버 (:3100)
pnpm lint                   # ESLint
pnpm type:check             # TypeScript 타입 체크
pnpm format:check           # Prettier
pnpm build                  # 프로덕션 빌드

# 백엔드
uv run trend-korea-api      # API 서버 (:8000)
uv run trend-korea-worker   # 스케줄러 워커
uv run pytest               # 테스트
uv run ruff check src/      # 린트
uv run ruff format src/     # 포매팅
uv run alembic upgrade head # DB 마이그레이션
```

### Project Structure

```
trend-korea/                     # 루트 (이 PRD가 위치)
├── trend-korea-api/             # 백엔드 (FastAPI)
│   ├── src/
│   │   ├── api/v1/              # 라우터 (13개)
│   │   ├── schemas/             # Pydantic 스키마
│   │   ├── crud/                # 비즈니스 로직
│   │   ├── sql/                 # 데이터 액세스
│   │   ├── models/              # ORM 모델 (9개 도메인)
│   │   ├── db/                  # DB 설정, 열거형, 파이프라인 모델
│   │   ├── scheduler/           # APScheduler 잡
│   │   ├── utils/               # 크롤러, 요약기, 파이프라인
│   │   └── core/                # 설정, 로깅
│   ├── migrations/              # Alembic
│   └── tests/
├── trend-korea-app/             # 프론트엔드 (Next.js)
│   └── src/
│       ├── app/                 # 라우팅
│       ├── features/            # 도메인 기능 (8개)
│       └── shared/              # 공유 모듈
└── trend-korea-admin/           # 어드민 (Next.js Fullstack)
    └── src/
        ├── app/                 # App Router (라우팅 + Route Handlers)
        ├── lib/                 # Prisma 클라이언트, 인증 등
        └── components/          # UI 컴포넌트
```

### Git Workflow

- 브랜치: `main` (프로덕션), `feature/*`, `fix/*`
- 커밋 메시지: 한국어, Conventional Commits 스타일
- PR: 기능 단위, 린트/테스트 통과 필수

---

## 15. MVP 범위 + 로드맵

### Phase 1: MVP

- [x] 메인 페이지 (속보, 타임라인 미니맵, 커뮤니티 핫, 검색순위, 트렌드, 주요 뉴스)
- [x] 타임라인 (일자별 사건 아카이브, 무한 스크롤, 필터/정렬, 상세 모달)
- [x] 이슈 추적 (목록, 상세, 트리거 타임라인, 추적 등록)
- [x] 커뮤니티 (게시글 CRUD, 댓글/대댓글, 추천/비추천, 탭/정렬)
- [x] 검색 (통합 검색, 고급 필터, 탭별 결과)
- [x] 내 추적 (추적 이슈/저장 사건 모아보기)
- [x] MY 페이지 (프로필, SNS 연동, 계정 관리)
- [x] 인증 (이메일 + SNS 로그인/회원가입)
- [x] 백엔드 API 66개 엔드포인트
- [x] 뉴스 수집 파이프라인 (키워드 → 크롤링 → 분류 → 요약 → 피드)
- [x] 스케줄러 워커 (6개 잡)
- [ ] 어드민: 사건/이슈/트리거 CRUD → trend-korea-admin 프로젝트로 이관
- [ ] 어드민: 태그/출처 CRUD → trend-korea-admin 프로젝트로 이관

### Phase 2: 개선

- [ ] 어드민 대시보드 (지표, 최근 활동) → trend-korea-admin 프로젝트로 이관
- [ ] 어드민 사용자 관리 (역할 변경, 정지) → trend-korea-admin 프로젝트로 이관
- [ ] 어드민 커뮤니티 모더레이션 (신고 시스템) → trend-korea-admin 프로젝트로 이관
- [ ] 어드민 파이프라인 모니터링 UI → trend-korea-admin 프로젝트로 이관
- [ ] 실시간 피드 개선 (WebSocket/SSE)
- [ ] 알림 시스템 (푸시/이메일)
- [ ] 게시글 이미지 첨부
- [ ] 다크 모드 / 테마 설정
- [ ] 지표 스트립 (환율, 출산률 등)
- [ ] 입법 현황 요약

### Phase 3: 확장 (장기)

- [ ] AI 요약 (이슈 자동 요약)
- [ ] 팔로우 (유저 구독)
- [ ] 프리미엄 멤버십
- [ ] 오픈 API
- [ ] 데이터 내보내기 (CSV/JSON)
- [ ] 지표 상세 (시계열 그래프)
- [ ] 다국어 지원

---

## 16. 용어 사전

| 한글   | 영문      | 코드        | DB 테이블    | 설명                              |
| ------ | --------- | ----------- | ------------ | --------------------------------- |
| 사건   | Event     | `Event`     | `events`     | 특정 일자에 발생한 단일 이벤트    |
| 이슈   | Issue     | `Issue`     | `issues`     | 언론/SNS에서 지속 추적되는 주제   |
| 트리거 | Trigger   | `Trigger`   | `triggers`   | 이슈에 대한 새로운 업데이트       |
| 태그   | Tag       | `Tag`       | `tags`       | 사건/이슈 분류 라벨               |
| 출처   | Source    | `Source`    | `sources`    | 뉴스 기사, 공식 발표 등 참고 자료 |
| 게시글 | Post      | `Post`      | `posts`      | 커뮤니티 게시글                   |
| 댓글   | Comment   | `Comment`   | `comments`   | 게시글에 대한 댓글/대댓글         |
| 사용자 | User      | `User`      | `users`      | 서비스 사용자                     |
| 검색순위 | Search Ranking | `SearchRanking` | `search_rankings` | 인기 검색 키워드 순위    |

---

## 17. 미결 사항 / 추후 결정

| 항목 | 상태 | 비고 |
|------|------|------|
| 배포 인프라 (VPS vs Cloud Run 등) | 미결정 | MVP 완료 후 결정 |
| CI/CD 파이프라인 | 미결정 | GitHub Actions 유력 |
| 어드민 UI 구현 방식 | **결정됨** | `trend-korea-admin` 별도 Next.js 풀스택 앱 (Prisma + shadcn/ui) |
| 신고 시스템 데이터 모델 | **결정됨** | `trend-korea-admin/docs/PRD.md` Section 5.4 `reports` 테이블 참조 |
| Rate Limiting 정책 | 미결정 | 인증 엔드포인트 우선 적용 |
| 이미지 업로드 스토리지 | 미결정 | S3 / Cloudflare R2 등 |
| 모니터링/알림 도구 | 미결정 | Sentry, Grafana 등 |

---

> **완성도**: 코드베이스 실사 기반 통합 명세 (2026-03-09 기술 검증 반영)
