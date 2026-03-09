# Development Guidelines

## 프로젝트 개요

- **서비스명**: 트렌드 코리아 — 대한민국 사회 이슈/사건 추적/분석 서비스
- **구조**: Git Submodules 기반 모노레포 (루트 + 2개 서브모듈)
- **서브모듈**: 각각 독립된 GitHub 레포지토리, 루트에서 `.gitmodules`로 관리

| 디렉토리 | 역할 | 기술 스택 | 패키지 매니저 |
|----------|------|----------|-------------|
| `trend-korea-api/` | 백엔드 API | FastAPI + SQLAlchemy 2.0 (sync) + PostgreSQL 16 | uv |
| `trend-korea-app/` | 프론트엔드 | Next.js 16 (App Router) + React 19 + TypeScript 5.9 + Tailwind CSS 4 | pnpm |

### 도메인 용어 (코드에서 반드시 영문 사용)

| 한글 | 영문 (코드) | 설명 |
|------|-----------|------|
| 사건 | `Event` | 특정 일자에 발생한 단일 이벤트 |
| 이슈 | `Issue` | 언론/SNS에서 지속 추적되는 주제 |
| 트리거 | `Trigger` | 이슈에 대한 새로운 업데이트 |
| 태그 | `Tag` | 사건/이슈 분류 라벨 |
| 출처 | `Source` | 뉴스 기사, 공식 발표 등 참고 자료 |
| 게시글 | `Post` | 커뮤니티 게시글 |

---

## 프로젝트 아키텍처

### 백엔드 (trend-korea-api)

**레이어 기반 구조** — 도메인별 파일로 구성:

```
api/v1/{domain}.py → schemas/{domain}.py → crud/{domain}.py → sql/{domain}.py → models/{domain}.py
(라우터)              (스키마)                (비즈니스 로직)      (데이터 액세스)    (모델)
```

**디렉토리 구조**:

```
src/
├── api/v1/          # FastAPI 라우터 (도메인별 파일)
├── schemas/         # Pydantic V2 요청/응답 스키마
├── crud/            # 비즈니스 로직
├── sql/             # SQLAlchemy 쿼리 (데이터 액세스 계층)
├── models/          # SQLAlchemy 모델
├── db/              # DB 연결, 세션, 배럴 import (__init__.py)
├── core/            # config, security, exceptions, pagination, response, logging
├── scheduler/       # APScheduler 잡 (jobs/ 하위)
├── utils/           # 크롤러, 파이프라인, 요약기 등 유틸리티
│   ├── keyword_crawler/
│   ├── news_crawler/
│   ├── naver_news_crawler/
│   ├── news_summarizer/
│   ├── pipeline/
│   ├── product_crawler/
│   └── social/
├── main.py          # FastAPI 앱 엔트리포인트
└── worker_main.py   # 스케줄러 워커 엔트리포인트
```

**도메인 목록**: `auth`, `users`, `events`, `issues`, `triggers`, `community`, `search`, `tracking`, `home`, `tags`, `sources`

### 프론트엔드 (trend-korea-app)

**Features + Shared 패턴** — 의존성 방향: `app → features → shared` (역방향 금지)

```
src/
├── app/                    # Next.js App Router (라우팅 & features 조립만)
│   ├── (auth)/             # 인증 라우트 그룹 (login, register)
│   ├── (main)/             # 메인 라우트 그룹
│   │   ├── community/      # 커뮤니티
│   │   ├── issues/         # 이슈 추적
│   │   ├── mypage/         # 마이페이지
│   │   ├── search/         # 검색
│   │   ├── timeline/       # 타임라인
│   │   └── tracking/       # 내 추적
│   └── api/                # Route Handlers (BFF 프록시)
├── features/               # 도메인 기능 모듈
│   ├── {feature}/
│   │   ├── ui/             # UI 컴포넌트
│   │   ├── model/          # 타입, API 훅, 스키마
│   │   ├── utils/          # 유틸리티 (선택)
│   │   └── index.ts        # Public API (배럴 export)
├── shared/                 # 도메인 중립 공유 모듈
│   ├── layouts/            # 레이아웃 컴포넌트
│   ├── ui/                 # 공통 UI 컴포넌트
│   ├── lib/                # 라이브러리 래퍼
│   ├── utils/              # 유틸리티
│   ├── types/              # 공통 타입
│   └── styles/             # 글로벌 스타일
└── test/                   # 테스트 인프라 (mocks, setup, utils)
```

**Feature 모듈 목록**: `auth`, `community`, `home`, `issues`, `mypage`, `search`, `timeline`, `tracking`

---

## 코드 표준

### 백엔드 (Python)

- **린터/포매터**: ruff (line-length=100, target-version=py311)
- **Import**: `from src.{layer}.{domain} import ...`
- **네이밍**: snake_case (변수, 함수, 파일), PascalCase (클래스, 모델)
- **타입 힌트**: 모든 함수 파라미터와 반환값에 필수
- **환경변수**: `src/core/config.py`의 `Settings` 클래스에서 관리, 필수: `DATABASE_URL`

### 프론트엔드 (TypeScript)

- **Prettier**: printWidth=120, useTabs=true, singleQuote=false, semi=true, trailingComma=none
- **ESLint**: consistent-type-imports (inline: `import { type Foo }`), simple-import-sort
- **Import 규칙**:
  - 상대 경로 사용
  - named export 우선
  - 타입 import는 반드시 인라인: `import { type Foo } from "./bar"`
  - import 순서는 `simple-import-sort`가 자동 정렬
- **네이밍**: camelCase (변수, 함수), PascalCase (컴포넌트, 타입, 인터페이스)
- **파일명**: PascalCase (컴포넌트 `.tsx`), camelCase (유틸/훅 `.ts`)

---

## 기능 구현 표준

### 백엔드: 새 도메인 추가 절차

1. `src/models/{domain}.py` — SQLAlchemy 모델 정의
2. `src/db/__init__.py` — **반드시** 배럴 import 등록
3. `src/schemas/{domain}.py` — Pydantic V2 요청/응답 스키마
4. `src/sql/{domain}.py` — 데이터 액세스 함수
5. `src/crud/{domain}.py` — 비즈니스 로직 함수
6. `src/api/v1/{domain}.py` — FastAPI 라우터
7. `src/main.py` — 라우터 등록 (`app.include_router`)
8. Alembic 마이그레이션 생성: `uv run alembic revision --autogenerate -m "설명"`

### 백엔드: 기존 도메인 수정 절차

- 모델 필드 추가/변경 → Alembic 마이그레이션 필수
- 스키마 변경 시 → 해당 도메인의 `schemas/{domain}.py` 동시 수정
- API 응답 구조 변경 시 → 프론트엔드 `features/{feature}/model/` 타입도 동시 확인

### 프론트엔드: 새 Feature 추가 절차

1. `src/features/{feature}/index.ts` — Public API (배럴 export) 생성
2. `src/features/{feature}/ui/` — UI 컴포넌트 작성
3. `src/features/{feature}/model/` — 타입, API 훅 작성
4. `src/app/(main)/{route}/page.tsx` — 페이지에서 feature import
5. 외부에서 접근 시 **반드시** `index.ts`를 통해서만 import

### 프론트엔드: API Route Handler 추가 절차

1. `src/app/api/{path}/route.ts` — Route Handler 작성
2. 백엔드 API를 프록시하는 BFF 패턴 사용
3. 인증 토큰은 쿠키에서 읽어 백엔드로 전달

---

## 프레임워크/라이브러리 사용 표준

### 프론트엔드

| 라이브러리 | 용도 | 규칙 |
|-----------|------|------|
| `@kim-yeo-appweb-lab/ui` | UI 컴포넌트 라이브러리 | **모든 UI는 여기서 import** (HTML 요소 직접 사용 금지) |
| `@tanstack/react-query` | 서버 상태 관리 | useQuery/useMutation 사용, features/model/ 내에 훅 정의 |
| `ky` | HTTP 클라이언트 | fetch 래퍼, shared/lib/ 내에서 인스턴스 관리 |
| `react-hook-form` + `@hookform/resolvers` | 폼 관리 | Zod resolver 사용 |
| `zod` | 스키마 검증 | 폼 검증 스키마는 features/model/schemas/ 내에 정의 |
| `lucide-react` | 아이콘 | shadcn/ui와 호환되는 아이콘 세트 |
| `tailwindcss` v4 | 스타일링 | `dark:` 변형 사용, CSS 변수는 className에서 `var()` 사용 금지 |

### 백엔드

| 라이브러리 | 용도 | 규칙 |
|-----------|------|------|
| `FastAPI` | 웹 프레임워크 | 라우터는 `src/api/v1/`에 정의 |
| `SQLAlchemy 2.0` | ORM | **sync 모드만 사용** (async 금지) |
| `Pydantic V2` | 검증/직렬화 | 스키마는 `src/schemas/`에 정의, `model_config` 사용 |
| `Alembic` | DB 마이그레이션 | 모델 변경 시 반드시 마이그레이션 생성 |
| `APScheduler` | 스케줄러 | BlockingScheduler 사용, 잡은 `src/scheduler/jobs/`에 정의 |
| `python-jose` | JWT | 인증 토큰 생성/검증, `src/core/security.py`에서 관리 |

---

## 워크플로우 표준

### 검증 명령어

| 환경 | 명령어 | 설명 |
|------|--------|------|
| 백엔드 | `uv run pytest` | 테스트 실행 |
| 백엔드 | `uv run ruff check src/` | 린트 |
| 백엔드 | `uv run ruff format src/` | 포매팅 |
| 백엔드 | `uv run alembic upgrade head` | DB 마이그레이션 적용 |
| 프론트엔드 | `pnpm lint` | ESLint |
| 프론트엔드 | `pnpm type:check` | TypeScript 타입 체크 |
| 프론트엔드 | `pnpm format:check` | Prettier 확인 |
| 프론트엔드 | `pnpm build` | 프로덕션 빌드 |
| 프론트엔드 | `pnpm test:run` | Vitest 테스트 |

### 서버 실행

| 명령어 | 설명 |
|--------|------|
| `uv run trend-korea-api` | 백엔드 API 서버 (:8000) |
| `uv run trend-korea-worker` | 스케줄러 워커 |
| `pnpm dev` (trend-korea-app/) | 프론트엔드 개발 서버 (:3100) |

### CLI 유틸리티 (백엔드)

| 명령어 | 설명 |
|--------|------|
| `uv run trend-korea-crawl-keywords` | 키워드 크롤링 |
| `uv run trend-korea-crawl-news` | 뉴스 크롤링 |
| `uv run trend-korea-crawl-naver-news` | 네이버 뉴스 크롤링 |
| `uv run trend-korea-summarize-news` | 뉴스 요약 (OpenAI) |
| `uv run trend-korea-full-cycle` | 전체 파이프라인 실행 |
| `uv run trend-korea-crawl-products` | 상품 크롤링 |

---

## 핵심 파일 연동 규칙

### 반드시 동시 수정이 필요한 케이스

| 변경 사항 | 함께 수정할 파일 |
|----------|----------------|
| 새 SQLAlchemy 모델 추가 | `src/models/{domain}.py` + `src/db/__init__.py` (배럴 import 등록) |
| 새 API 라우터 추가 | `src/api/v1/{domain}.py` + `src/main.py` (include_router) |
| 모델 필드 변경 | `src/models/{domain}.py` + `src/schemas/{domain}.py` + Alembic 마이그레이션 |
| 백엔드 API 응답 변경 | `src/schemas/{domain}.py` + 프론트엔드 `features/{feature}/model/` 타입 |
| 새 프론트엔드 Feature | `src/features/{feature}/index.ts` (배럴 export) + `src/app/` (페이지) |
| 새 어드민 페이지 추가 | `src/app/(admin)/admin/{page}/page.tsx` + 사이드바 메뉴 항목 |
| 새 Route Handler | `src/app/api/{path}/route.ts` + 관련 feature의 API 훅 |

### 상세 문서 참조 가이드

- 백엔드 아키텍처 상세 → `trend-korea-api/agent_docs/architecture.md`
- 백엔드 코드 규칙 상세 → `trend-korea-api/agent_docs/conventions.md`
- 백엔드 전체 명령어 → `trend-korea-api/agent_docs/commands.md`
- 프론트엔드 아키텍처 상세 → `trend-korea-app/agent_docs/architecture.md`
- 프론트엔드 코드 규칙 상세 → `trend-korea-app/agent_docs/conventions.md`
- 프론트엔드 UI 가이드 → `trend-korea-app/agent_docs/ui-guidelines.md`
- 프론트엔드 PRD → `trend-korea-app/docs/PRD.md`
- 개발 로드맵 → `docs/ROADMAP.md`

---

## AI 의사결정 기준

### 작업 영역 판단

```
요청에 API/엔드포인트/DB/모델/크롤러 포함?
  → 백엔드 (trend-korea-api/)

요청에 UI/페이지/컴포넌트/스타일 포함?
  → 프론트엔드 (trend-korea-app/)

요청에 양쪽 모두 포함?
  → 백엔드 먼저 (API 계약 확정) → 프론트엔드 연동
```

### 새 기능 구현 시 판단 트리

```
1. 기존 도메인에 속하는가?
   YES → 해당 도메인 파일들에 추가
   NO  → 새 도메인 파일 세트 생성 (기능 구현 표준 참조)

2. DB 스키마 변경이 필요한가?
   YES → 모델 수정 + Alembic 마이그레이션 필수
   NO  → 진행

3. 프론트엔드 타입 변경이 필요한가?
   YES → features/{feature}/model/ 타입 동시 수정
   NO  → 진행
```

### 우선순위

1. **타입 안전성**: 백엔드 Pydantic 스키마 ↔ 프론트엔드 TypeScript 타입 일치 보장
2. **레이어 준수**: 레이어 간 건너뛰기 금지 (예: 라우터에서 직접 SQL 호출 금지)
3. **테스트**: Phase 3 이후 TDD 필수 (Red-Green-Refactor)

---

## 금지 사항

### 공통

- **임시 우회 금지**: setTimeout, 플래그, 무한 재시도 등으로 문제 회피 금지 → 근본 원인 분석 후 해결
- **서브모듈 간 직접 import 금지**: trend-korea-api와 trend-korea-app 간 코드 직접 참조 불가
- **환경변수 하드코딩 금지**: 반드시 config/환경변수 파일에서 관리

### 백엔드

- **async SQLAlchemy 사용 금지**: sync 모드만 사용
- **라우터에서 직접 SQL 쿼리 금지**: 반드시 `sql/` → `crud/` 레이어를 거쳐야 함
- **db/__init__.py 배럴 import 누락 금지**: 새 모델 추가 시 반드시 등록
- **Alembic 마이그레이션 없는 모델 변경 금지**

### 프론트엔드

- **HTML 요소 직접 사용 금지**: `<button>`, `<input>` 등 → `@kim-yeo-appweb-lab/ui`에서 import
- **Feature 간 직접 import 금지**: 반드시 해당 feature의 `index.ts`를 통해서만 접근
- **역방향 의존성 금지**: `shared`에서 `features` import 금지, `features`에서 `app` import 금지
- **default export 사용 금지**: named export 사용 (Next.js 페이지 컴포넌트 제외)
- **CSS-in-JS 사용 금지**: Tailwind CSS만 사용
- **`var()` className 내 사용 금지**: Tailwind CSS 4에서 CSS 변수를 className에 직접 넣지 않음
- **`any` 타입 사용 금지**: `unknown` + 타입 가드 사용
