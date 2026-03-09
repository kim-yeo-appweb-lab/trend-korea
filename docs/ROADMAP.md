# ROADMAP

> PRD 기반 개발 로드맵
> 생성일: 2026-03-09
> 기반 PRD: `docs/PRD.md`

## 개요

대한민국 사건/이슈를 시간순으로 기록하고, 이슈를 지속 추적하여 제공하는 정보 아카이브 서비스의 개발 로드맵.

### 기술 스택

| 레이어 | 기술 |
|--------|------|
| 프론트엔드 | Next.js 16 (App Router) + React 19 + TypeScript 5.9 + Tailwind CSS 4 + shadcn/ui |
| 백엔드 | FastAPI + SQLAlchemy 2.0 (sync) + Pydantic V2 + PostgreSQL 16 |
| 스케줄러 | APScheduler (BlockingScheduler) |
| AI/LLM | OpenAI API (뉴스 요약) |
| 패키지 매니저 | pnpm (프론트엔드) / uv (백엔드) |
| 린터/포매터 | ESLint 9 + Prettier (프론트엔드) / ruff (백엔드) |
| 테스트 | pytest (백엔드) |

### 테스트 전략

- **백엔드**: pytest 기반, 18개 테스트 파일 존재 (도메인별 테스트)
- **프론트엔드**: 테스트 프레임워크 미설정 (Phase 3 이후 도입 검토)
- **테스트 트로피**: Unit 30% / Integration 40% / E2E 10% / Static Analysis 20%
- **Phase 3 이후**: TDD 필수 (Red-Green-Refactor)

### 현재 상태

MVP의 사용자 서비스는 대부분 구현 완료 (백엔드 64개 API, 프론트엔드 8개 feature 모듈). 어드민 기능은 백엔드 API만 존재하고 프론트엔드 UI는 미구현.

---

## Phase 요약

| Phase | 이름 | 설명 | 예상 기간 | TDD | 상태 |
|-------|------|------|-----------|-----|------|
| 1 | MVP 완성 - 프로젝트 골격 | 어드민 모듈 골격 설정, 인프라 기반 | 1주 | - | ⬜ |
| 2 | MVP 완성 - 어드민 UI/UX | 어드민 CRUD 화면 (더미 데이터) | 1.5주 | 선택 | ⬜ |
| 3 | MVP 완성 - 어드민 기능 연동 | 기존 admin API와 UI 연동 | 1주 | 필수 | ⬜ |
| 4 | Phase 2 개선 - 어드민 고도화 | 대시보드, 사용자 관리, 모더레이션, 파이프라인 모니터링 | 3주 | 필수 | ⬜ |
| 5 | Phase 2 개선 - 사용자 서비스 강화 | 실시간 피드, 알림, 이미지 첨부, 다크모드, 지표 | 4주 | 필수 | ⬜ |
| 6 | Phase 3 확장 | AI 요약, 팔로우, 프리미엄, 오픈 API, 다국어 | 장기 | 필수 | ⬜ |

---

## 완료된 기능 (MVP 사용자 서비스)

> 아래 기능은 코드베이스 검증 완료 (2026-03-09 기준)

- [x] `DONE-001` 메인 페이지: 속보 피드 (FR-M1), 타임라인 미니맵 (FR-M2), 커뮤니티 핫 (FR-M3), 검색순위 (FR-M4), 트렌드 (FR-M5), 주요 뉴스 (FR-M6), 커뮤니티 미디어 (FR-M7)
- [x] `DONE-002` 타임라인: 날짜 선택기 (FR-T1), 필터/정렬 (FR-T2, FR-T3), 사건 카드 (FR-T4), 무한 스크롤 (FR-T5), 사건 상세 모달 (FR-T6)
- [x] `DONE-003` 이슈 추적: 목록/필터/정렬 (FR-I1, FR-I2), 이슈 카드 (FR-I3), 이슈 상세 (FR-I4), 추적 등록/해제 (FR-I5)
- [x] `DONE-004` 커뮤니티: 탭/정렬 (FR-C1, FR-C5), 글쓰기 (FR-C3), 게시글 상세/댓글 (FR-C4)
- [x] `DONE-005` 검색: 통합 검색 (FR-S1~S5)
- [x] `DONE-006` 내 추적: 추적/저장 모아보기 (FR-M1~M4)
- [x] `DONE-007` MY 페이지: 프로필/계정 관리 (FR-MY1~MY4)
- [x] `DONE-008` 인증: 이메일 + SNS 로그인/회원가입
- [x] `DONE-009` 백엔드 API 64개 엔드포인트 (13개 라우터)
- [x] `DONE-010` 뉴스 수집 파이프라인 (키워드 -> 크롤링 -> 분류 -> 요약 -> 피드)
- [x] `DONE-011` 스케줄러 워커 (6개 잡)
- [x] `DONE-012` 백엔드 테스트 (18개 테스트 파일)

---

## Phase 1: MVP 완성 - 프로젝트 골격

**목표**: 어드민 모듈의 프론트엔드 기반 구조를 설정한다
**예상 기간**: 1주
**선행 조건**: 없음 (기존 프로젝트 위에 확장)

#### 태스크

- [ ] `P1-001` 어드민 라우트 그룹 생성
  - **설명**: Next.js App Router에 `(admin)` 라우트 그룹을 추가하고, 어드민 전용 레이아웃을 구성한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/app/(admin)/layout.tsx` — 어드민 전용 레이아웃 (사이드바 네비게이션)
    - `src/app/(admin)/admin/page.tsx` — 어드민 메인 (리다이렉트 또는 대시보드)
    - `src/app/(admin)/admin/events/page.tsx` — 사건 관리 페이지 (빈 껍데기)
    - `src/app/(admin)/admin/issues/page.tsx` — 이슈 관리 페이지 (빈 껍데기)
    - `src/app/(admin)/admin/tags/page.tsx` — 태그 관리 페이지 (빈 껍데기)
    - `src/app/(admin)/admin/sources/page.tsx` — 출처 관리 페이지 (빈 껍데기)
  - **예상 소요**: 1일
  - **의존성**: 없음

- [ ] `P1-002` 어드민 feature 모듈 생성
  - **설명**: `features/admin/` 모듈을 생성하고, 하위 도메인별 디렉토리 구조를 설정한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/index.ts` — Public API (배럴 export)
    - `src/features/admin/ui/` — UI 컴포넌트 디렉토리
    - `src/features/admin/model/` — 타입, 스키마, API 훅 디렉토리
    - `src/features/admin/utils/` — 유틸리티 디렉토리
  - **예상 소요**: 0.5일
  - **의존성**: 없음

- [ ] `P1-003` 어드민 권한 가드 구현
  - **설명**: `role=admin` 사용자만 어드민 페이지에 접근할 수 있도록 미들웨어 또는 레이아웃 수준 가드를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/AdminGuard.tsx` — 권한 체크 래퍼 컴포넌트
    - `src/app/(admin)/layout.tsx` 내에서 `AdminGuard` 적용
  - **예상 소요**: 1일
  - **의존성**: `P1-001`

- [ ] `P1-004` 어드민 사이드바 네비게이션 컴포넌트
  - **설명**: 어드민 페이지 간 이동을 위한 사이드바 네비게이션을 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/AdminSidebar.tsx` — 사이드바 컴포넌트
    - 메뉴 항목: 사건 관리, 이슈 관리, 태그 관리, 출처 관리
    - shadcn/ui 컴포넌트 활용
  - **예상 소요**: 1일
  - **의존성**: `P1-001`, `P1-002`

#### 완료 기준
- `/admin` 경로 접근 시 어드민 레이아웃이 렌더링된다
- `role=admin`이 아닌 사용자는 접근이 차단된다
- 사이드바에서 각 관리 페이지로 이동할 수 있다
- `pnpm lint` 및 `pnpm type:check` 통과

---

## Phase 2: MVP 완성 - 어드민 UI/UX 레이아웃

**목표**: 더미 데이터로 어드민 CRUD 화면을 구성하여 UI/UX를 검증한다
**예상 기간**: 1.5주
**선행 조건**: Phase 1

> 이 Phase의 모든 UI는 더미/하드코딩 데이터로 동작한다.
> 실제 API 연동은 Phase 3에서 수행한다.

#### 태스크

- [ ] `P2-001` 사건 관리 목록 UI
  - **설명**: 사건 목록을 테이블 형태로 표시하고, 검색/필터/페이지네이션 UI를 구성한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/events/EventListPage.tsx` — 사건 목록 페이지
    - `src/features/admin/ui/events/EventTable.tsx` — 테이블 컴포넌트
    - `src/features/admin/ui/events/EventFilters.tsx` — 필터 컴포넌트
  - **더미 데이터**: `{ id, title, occurred_at, importance, verification_status, source_count, tags[] }`
  - **예상 소요**: 2일
  - **의존성**: `P1-004`

- [ ] `P2-002` 사건 생성/수정 폼 UI
  - **설명**: 사건 생성 및 수정을 위한 폼 UI를 구현한다 (Zod 스키마 포함)
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/events/EventForm.tsx` — 폼 컴포넌트 (생성/수정 공용)
    - `src/features/admin/model/schemas/eventSchema.ts` — Zod 검증 스키마
    - 필드: 제목, 요약, 발생 일시, 중요도(select), 검증 상태(select), 태그(multi-select), 출처(동적 추가)
  - **더미 데이터**: 수정 시 기존 사건 데이터 하드코딩
  - **예상 소요**: 2일
  - **의존성**: `P2-001`

- [ ] `P2-003` 이슈 관리 목록 UI
  - **설명**: 이슈 목록을 테이블 형태로 표시한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/issues/IssueListPage.tsx` — 이슈 목록 페이지
    - `src/features/admin/ui/issues/IssueTable.tsx` — 테이블 컴포넌트
  - **더미 데이터**: `{ id, title, status, tracker_count, latest_trigger_at, tags[] }`
  - **예상 소요**: 1일
  - **의존성**: `P1-004`

- [ ] `P2-004` 이슈 생성/수정 폼 UI
  - **설명**: 이슈 생성 및 수정 폼을 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/issues/IssueForm.tsx` — 폼 컴포넌트
    - `src/features/admin/model/schemas/issueSchema.ts` — Zod 검증 스키마
    - 필드: 제목, 설명, 상태(select), 태그(multi-select), 관련 사건(검색/선택)
  - **더미 데이터**: 수정 시 기존 이슈 데이터 하드코딩
  - **예상 소요**: 2일
  - **의존성**: `P2-003`

- [ ] `P2-005` 트리거 관리 UI (이슈 상세 내부)
  - **설명**: 이슈 상세 페이지 내에서 트리거 목록 표시 및 생성/수정/삭제 UI를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/issues/TriggerSection.tsx` — 트리거 섹션
    - `src/features/admin/ui/issues/TriggerForm.tsx` — 트리거 폼 (모달)
    - 필드: 요약, 발생 일시, 유형(select), 출처(동적 추가)
  - **더미 데이터**: `{ id, summary, occurred_at, type, sources[] }`
  - **예상 소요**: 1.5일
  - **의존성**: `P2-004`

- [ ] `P2-006` 태그 관리 UI
  - **설명**: 태그 목록, 생성, 수정, 삭제 UI를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/tags/TagListPage.tsx` — 태그 목록 페이지
    - `src/features/admin/ui/tags/TagForm.tsx` — 태그 생성/수정 폼 (인라인 또는 모달)
    - `src/features/admin/model/schemas/tagSchema.ts` — Zod 검증 스키마
    - 필드: 이름, 유형(category/region), slug
  - **더미 데이터**: `{ id, name, type, slug }`
  - **예상 소요**: 1일
  - **의존성**: `P1-004`

- [ ] `P2-007` 출처 관리 UI
  - **설명**: 출처 목록 및 추가/삭제 UI를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/sources/SourceListPage.tsx` — 출처 목록 페이지
    - `src/features/admin/ui/sources/SourceForm.tsx` — 출처 추가 폼
    - 필드: URL, 제목, 매체명, 발행일, 연결 엔티티(event/issue/trigger 선택)
  - **더미 데이터**: `{ id, url, title, publisher, published_at, entity_type, entity_id }`
  - **예상 소요**: 1일
  - **의존성**: `P1-004`

- [ ] `P2-008` 삭제 확인 다이얼로그 공통 컴포넌트
  - **설명**: CRUD 삭제 시 사용할 확인 다이얼로그를 공통 컴포넌트로 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/ui/common/DeleteConfirmDialog.tsx` — shadcn/ui AlertDialog 활용
  - **예상 소요**: 0.5일
  - **의존성**: `P1-002`

#### 완료 기준
- 모든 어드민 화면이 더미 데이터로 렌더링된다
- 폼 입력 시 Zod 클라이언트 검증이 동작한다
- 테이블 정렬/필터/페이지네이션 UI가 동작한다 (더미 데이터 기준)
- 반응형 레이아웃이 데스크탑/태블릿에서 동작한다
- `pnpm lint` 및 `pnpm type:check` 통과

---

## Phase 3: MVP 완성 - 어드민 기능 연동

**목표**: Phase 2에서 구성한 UI를 기존 백엔드 admin API와 연동한다
**예상 기간**: 1주
**선행 조건**: Phase 2
**TDD**: 필수 (Red-Green-Refactor)

#### 태스크

- [ ] `P3-001` 어드민 API 훅 구현 (React Query)
  - **설명**: 사건/이슈/트리거/태그/출처 CRUD API 호출을 위한 React Query 훅을 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/admin/model/api/eventApi.ts` — 사건 CRUD API 훅
    - `src/features/admin/model/api/issueApi.ts` — 이슈 CRUD API 훅
    - `src/features/admin/model/api/triggerApi.ts` — 트리거 CRUD API 훅
    - `src/features/admin/model/api/tagApi.ts` — 태그 CRUD API 훅
    - `src/features/admin/model/api/sourceApi.ts` — 출처 CRUD API 훅
  - **테스트 (TDD)**:
    - 🔴 각 API 훅이 올바른 엔드포인트를 호출하는지 테스트 작성 (실패)
    - 🟢 React Query `useMutation`/`useQuery` 훅 구현으로 테스트 통과
    - 🔵 공통 에러 핸들링 로직 추출, 타입 안전성 강화
  - **예상 소요**: 2일
  - **의존성**: `P2-001` ~ `P2-007`

- [ ] `P3-002` 사건 CRUD 연동
  - **설명**: 사건 목록/생성/수정/삭제를 실제 API와 연동한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `EventListPage.tsx` — 더미 데이터를 `useQuery` 호출로 교체
    - `EventForm.tsx` — 폼 제출을 `useMutation` 호출로 교체
    - 삭제 시 `DeleteConfirmDialog` + API 호출 연동
  - **테스트 (TDD)**:
    - 🔴 사건 생성 시 `POST /api/v1/events` 호출 및 목록 갱신 테스트
    - 🟢 `useMutation` + `queryClient.invalidateQueries` 구현
    - 🔵 낙관적 업데이트 적용 여부 검토
  - **API 엔드포인트**: `GET/POST /events`, `PATCH/DELETE /events/{id}`
  - **예상 소요**: 1.5일
  - **의존성**: `P3-001`

- [ ] `P3-003` 이슈/트리거 CRUD 연동
  - **설명**: 이슈 목록/생성/수정/삭제 및 트리거 CRUD를 실제 API와 연동한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `IssueListPage.tsx`, `IssueForm.tsx` — API 연동
    - `TriggerSection.tsx`, `TriggerForm.tsx` — API 연동
  - **테스트 (TDD)**:
    - 🔴 이슈 생성 시 관련 사건 연결이 올바르게 전송되는지 테스트
    - 🟢 이슈-사건 연결 로직 구현
    - 🔵 이슈-트리거 연쇄 갱신 로직 정리
  - **API 엔드포인트**: `GET/POST /issues`, `PATCH/DELETE /issues/{id}`, `GET/POST /issues/{id}/triggers`, `PATCH/DELETE /triggers/{id}`
  - **예상 소요**: 1.5일
  - **의존성**: `P3-001`

- [ ] `P3-004` 태그/출처 CRUD 연동
  - **설명**: 태그 및 출처 CRUD를 실제 API와 연동한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `TagListPage.tsx`, `TagForm.tsx` — API 연동
    - `SourceListPage.tsx`, `SourceForm.tsx` — API 연동
  - **테스트 (TDD)**:
    - 🔴 태그 생성 시 slug 자동 생성 검증 테스트
    - 🟢 태그/출처 CRUD mutation 구현
    - 🔵 태그 캐시 전략 최적화 (여러 페이지에서 참조)
  - **API 엔드포인트**: `GET/POST /tags`, `PATCH/DELETE /tags/{id}`, `GET/POST /sources`, `DELETE /sources/{id}`
  - **예상 소요**: 1일
  - **의존성**: `P3-001`

- [ ] `P3-005` 에러 핸들링 및 토스트 알림
  - **설명**: CRUD 작업 성공/실패 시 사용자 피드백을 제공한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - 성공 시 토스트 알림 (shadcn/ui Toast)
    - 실패 시 에러 메시지 표시
    - 403 응답 시 권한 부족 안내
  - **테스트 (TDD)**:
    - 🔴 API 에러 시 적절한 에러 메시지가 표시되는지 테스트
    - 🟢 에러 핸들러 구현
    - 🔵 에러 타입별 메시지 매핑 정리
  - **예상 소요**: 1일
  - **의존성**: `P3-002`

#### 완료 기준
- 어드민에서 사건/이슈/트리거/태그/출처 CRUD가 실제 DB와 연동된다
- 모든 CRUD 작업에 성공/실패 피드백이 제공된다
- 기존 백엔드 테스트 (`uv run pytest`) 통과
- `pnpm lint`, `pnpm type:check` 통과

---

## Phase 4: 어드민 고도화

**목표**: 어드민 대시보드, 사용자 관리, 커뮤니티 모더레이션, 파이프라인 모니터링을 구현한다
**예상 기간**: 3주
**선행 조건**: Phase 3
**TDD**: 필수 (Red-Green-Refactor)

#### Phase 4-1: 어드민 대시보드 (FR-AD1)

- [ ] `P4-001` 대시보드 통계 API 구현
  - **설명**: 어드민 대시보드에 필요한 통계 API를 백엔드에 추가한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `src/api/v1/admin.py` — 어드민 전용 라우터
    - `src/schemas/admin.py` — 응답 스키마
    - `src/crud/admin.py` — 통계 집계 로직
    - `src/sql/admin.py` — 집계 쿼리
    - 엔드포인트: `GET /api/v1/admin/dashboard/stats` (오늘 사건/트리거/이슈/사용자 수)
    - 엔드포인트: `GET /api/v1/admin/dashboard/recent-jobs` (최근 잡 실행 이력)
  - **테스트 (TDD)**:
    - 🔴 `/admin/dashboard/stats` 호출 시 올바른 집계 결과 반환 테스트
    - 🟢 SQL 집계 쿼리 및 라우터 구현
    - 🔵 쿼리 성능 최적화 (인덱스 활용)
  - **예상 소요**: 2일
  - **의존성**: `P3-005`

- [ ] `P4-002` 대시보드 UI 구현
  - **설명**: 어드민 대시보드 페이지를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/app/(admin)/admin/page.tsx` — 대시보드 페이지
    - `src/features/admin/ui/dashboard/StatsCards.tsx` — 통계 카드
    - `src/features/admin/ui/dashboard/RecentJobsTable.tsx` — 최근 잡 실행 테이블
  - **테스트 (TDD)**:
    - 🔴 통계 API 응답이 올바르게 카드에 렌더링되는지 테스트
    - 🟢 API 훅 + UI 컴포넌트 구현
    - 🔵 자동 갱신 (polling) 적용
  - **예상 소요**: 2일
  - **의존성**: `P4-001`

#### Phase 4-2: 사용자 관리 (FR-AD4)

- [ ] `P4-003` 사용자 관리 API 확장
  - **설명**: 사용자 목록 조회, 역할 변경, 정지/복원 API를 추가한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `src/api/v1/users.py` — 어드민 전용 엔드포인트 추가
    - `GET /api/v1/users` — 사용자 목록 (admin 전용, 필터/페이지네이션)
    - `PATCH /api/v1/users/{id}/role` — 역할 변경
    - `PATCH /api/v1/users/{id}/status` — 정지/복원 (`is_active` 토글)
  - **테스트 (TDD)**:
    - 🔴 비어드민 사용자가 역할 변경 API 호출 시 403 반환 테스트
    - 🟢 권한 체크 + CRUD 로직 구현
    - 🔵 자기 자신의 역할 변경 방지 로직 추가
  - **예상 소요**: 2일
  - **의존성**: `P3-005`

- [ ] `P4-004` 사용자 관리 UI 구현
  - **설명**: 사용자 목록, 역할 변경, 정지/복원 UI를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/app/(admin)/admin/users/page.tsx` — 사용자 관리 페이지
    - `src/features/admin/ui/users/UserListPage.tsx` — 사용자 목록
    - `src/features/admin/ui/users/UserRoleDialog.tsx` — 역할 변경 다이얼로그
    - 사이드바에 "사용자 관리" 메뉴 추가
  - **테스트 (TDD)**:
    - 🔴 역할 변경 다이얼로그에서 확인 클릭 시 API 호출 테스트
    - 🟢 UI 컴포넌트 + API 훅 연동
    - 🔵 목록 필터링/검색 기능 추가
  - **예상 소요**: 2일
  - **의존성**: `P4-003`

#### Phase 4-3: 커뮤니티 모더레이션 (FR-AD5)

- [ ] `P4-005` 신고 시스템 데이터 모델 및 API
  - **설명**: 게시글/댓글 신고 기능을 위한 데이터 모델과 API를 구현한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `src/models/report.py` — `Report` 모델 (`entity_type`, `entity_id`, `reporter_id`, `reason`, `status`)
    - `src/db/__init__.py` — 배럴 import 등록
    - Alembic 마이그레이션 파일 생성
    - `POST /api/v1/posts/{id}/report` — 게시글 신고
    - `POST /api/v1/comments/{id}/report` — 댓글 신고
    - `GET /api/v1/admin/reports` — 신고 목록 (admin)
    - `PATCH /api/v1/admin/reports/{id}` — 신고 처리 (admin)
  - **테스트 (TDD)**:
    - 🔴 동일 사용자가 같은 게시글을 중복 신고 시 409 반환 테스트
    - 🟢 신고 모델 + CRUD + API 구현
    - 🔵 신고 사유 enum 정의, 처리 상태 관리
  - **예상 소요**: 3일
  - **의존성**: `P3-005`

- [ ] `P4-006` 모더레이션 UI 구현
  - **설명**: 신고된 콘텐츠 검토 및 처리 UI를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/app/(admin)/admin/reports/page.tsx` — 신고 관리 페이지
    - `src/features/admin/ui/reports/ReportListPage.tsx` — 신고 목록
    - `src/features/admin/ui/reports/ReportActionDialog.tsx` — 처리 다이얼로그
    - 사이드바에 "신고 관리" 메뉴 추가 (미처리 건수 뱃지)
  - **테스트 (TDD)**:
    - 🔴 신고 처리 시 해당 게시글/댓글 삭제 옵션 동작 테스트
    - 🟢 UI + API 연동 구현
    - 🔵 신고 상태 필터링 UX 개선
  - **예상 소요**: 2일
  - **의존성**: `P4-005`

#### Phase 4-4: 파이프라인 모니터링 (FR-AD6)

- [ ] `P4-007` 파이프라인 모니터링 API
  - **설명**: 뉴스 수집 파이프라인 모니터링 API를 구현한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `GET /api/v1/admin/pipeline/stats` — 파이프라인 통계 (수집 기사 수, 요약 수, 분류 결과)
    - `GET /api/v1/admin/pipeline/keyword-states` — 키워드 상태 목록
    - `POST /api/v1/admin/pipeline/trigger-collect` — 수동 뉴스 수집 트리거
  - **테스트 (TDD)**:
    - 🔴 수동 수집 트리거 API 호출 시 잡이 큐에 등록되는지 테스트
    - 🟢 API + 스케줄러 연동 구현
    - 🔵 잡 실행 상태 실시간 조회 방법 검토
  - **예상 소요**: 2일
  - **의존성**: `P3-005`

- [ ] `P4-008` 파이프라인 모니터링 UI
  - **설명**: 파이프라인 모니터링 대시보드를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/app/(admin)/admin/pipeline/page.tsx` — 파이프라인 모니터링 페이지
    - `src/features/admin/ui/pipeline/PipelineStats.tsx` — 통계 카드
    - `src/features/admin/ui/pipeline/KeywordStateTable.tsx` — 키워드 상태 테이블
    - `src/features/admin/ui/pipeline/ManualTriggerButton.tsx` — 수동 수집 트리거
    - 사이드바에 "파이프라인" 메뉴 추가
  - **테스트 (TDD)**:
    - 🔴 수동 수집 트리거 버튼 클릭 시 확인 다이얼로그 + API 호출 테스트
    - 🟢 UI 컴포넌트 + API 연동 구현
    - 🔵 자동 갱신 (polling) 적용
  - **예상 소요**: 2일
  - **의존성**: `P4-007`

#### 완료 기준
- 어드민 대시보드에서 주요 지표를 확인할 수 있다
- 사용자 역할 변경 및 정지/복원이 동작한다
- 신고된 콘텐츠를 검토하고 처리할 수 있다
- 파이프라인 실행 이력을 확인하고 수동 수집을 트리거할 수 있다
- 모든 백엔드 테스트 통과 (`uv run pytest`)
- 모든 프론트엔드 린트/타입 체크 통과

---

## Phase 5: 사용자 서비스 강화

**목표**: 실시간 피드, 알림, 이미지 첨부, 다크 모드, 지표 등 사용자 경험을 개선한다
**예상 기간**: 4주
**선행 조건**: Phase 3 (MVP 완성)
**TDD**: 필수 (Red-Green-Refactor)

#### Phase 5-1: 실시간 피드 개선 (FR-F1)

- [ ] `P5-001` SSE 기반 실시간 피드 백엔드
  - **설명**: 기존 polling 방식의 피드를 SSE(Server-Sent Events)로 전환한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `src/api/v1/feed.py` — SSE 엔드포인트 추가 (`GET /api/v1/feed/stream`)
    - FastAPI `StreamingResponse` 활용
    - 피드 이벤트 타입: `breaking`, `major`, `update`
  - **테스트 (TDD)**:
    - 🔴 SSE 연결 시 올바른 Content-Type 헤더 반환 테스트
    - 🟢 SSE 스트림 구현
    - 🔵 연결 관리 (heartbeat, 타임아웃) 추가
  - **예상 소요**: 2일
  - **의존성**: 없음

- [ ] `P5-002` 실시간 피드 프론트엔드 연동
  - **설명**: SSE 클라이언트를 구현하여 실시간 피드를 표시한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/home/model/api/useFeedStream.ts` — SSE 훅
    - `src/features/home/ui/LiveFeedSection.tsx` — 실시간 피드 UI 업데이트
  - **테스트 (TDD)**:
    - 🔴 SSE 이벤트 수신 시 피드 목록에 새 항목이 추가되는지 테스트
    - 🟢 EventSource API 래퍼 + React 상태 연동
    - 🔵 재연결 로직, 에러 핸들링 개선
  - **예상 소요**: 2일
  - **의존성**: `P5-001`

#### Phase 5-2: 알림 시스템

- [ ] `P5-003` 알림 데이터 모델 및 API
  - **설명**: 사용자 알림 시스템의 데이터 모델과 API를 구현한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `src/models/notification.py` — `Notification` 모델
    - Alembic 마이그레이션
    - `GET /api/v1/users/me/notifications` — 알림 목록
    - `PATCH /api/v1/users/me/notifications/{id}/read` — 읽음 처리
    - `POST /api/v1/users/me/notifications/read-all` — 전체 읽음
    - 트리거 생성 시 해당 이슈 추적자에게 알림 생성
  - **테스트 (TDD)**:
    - 🔴 트리거 생성 시 추적자에게 알림이 생성되는지 테스트
    - 🟢 알림 생성 로직 + API 구현
    - 🔵 알림 일괄 처리 성능 최적화
  - **예상 소요**: 3일
  - **의존성**: 없음

- [ ] `P5-004` 알림 UI 구현
  - **설명**: 헤더에 알림 벨 아이콘과 알림 목록 드롭다운을 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/shared/layouts/NotificationBell.tsx` — 알림 벨 컴포넌트
    - `src/shared/layouts/NotificationDropdown.tsx` — 알림 드롭다운
    - 미읽은 알림 수 뱃지 표시
  - **테스트 (TDD)**:
    - 🔴 미읽은 알림이 있을 때 뱃지에 숫자가 표시되는지 테스트
    - 🟢 알림 API 훅 + UI 구현
    - 🔵 SSE 기반 실시간 알림 수신 연동
  - **예상 소요**: 2일
  - **의존성**: `P5-003`

#### Phase 5-3: 게시글 이미지 첨부

- [ ] `P5-005` 이미지 업로드 API
  - **설명**: 게시글에 이미지를 첨부할 수 있는 업로드 API를 구현한다
  - **영역**: 백엔드
  - **구현 사항**:
    - `POST /api/v1/uploads/images` — 이미지 업로드 (반환: URL)
    - 이미지 저장소: 로컬 스토리지 (추후 S3/R2 전환 가능하도록 추상화)
    - 파일 크기 제한: 5MB, 허용 형식: JPEG/PNG/WebP
  - **테스트 (TDD)**:
    - 🔴 5MB 초과 파일 업로드 시 413 반환 테스트
    - 🟢 파일 검증 + 저장 로직 구현
    - 🔵 스토리지 어댑터 패턴 적용 (로컬/S3 교체 가능)
  - **예상 소요**: 2일
  - **의존성**: 없음
  - [PRD 확인 필요] 이미지 저장소 결정 (S3 / Cloudflare R2 등)

- [ ] `P5-006` 마크다운 에디터 이미지 삽입 UI
  - **설명**: 커뮤니티 글쓰기 에디터에서 이미지 드래그앤드롭/붙여넣기를 지원한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - 글쓰기 에디터에 이미지 업로드 버튼 추가
    - 드래그앤드롭 / 클립보드 붙여넣기 지원
    - 업로드 중 로딩 상태 표시
    - 마크다운 `![alt](url)` 형식으로 삽입
  - **테스트 (TDD)**:
    - 🔴 이미지 업로드 성공 시 마크다운 이미지 태그가 에디터에 삽입되는지 테스트
    - 🟢 업로드 핸들러 + 에디터 연동 구현
    - 🔵 이미지 미리보기, 삭제 기능 추가
  - **예상 소요**: 2일
  - **의존성**: `P5-005`

#### Phase 5-4: 다크 모드 / 테마

- [ ] `P5-007` 다크 모드 구현
  - **설명**: 시스템 설정 연동 + 수동 전환 가능한 다크 모드를 구현한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - Tailwind CSS 4 `dark:` 변형 활용
    - 테마 전환 컴포넌트 (헤더에 배치)
    - `localStorage`에 사용자 선택 저장
    - 전체 shadcn/ui 컴포넌트 다크 모드 대응
  - **테스트 (TDD)**:
    - 🔴 테마 전환 시 `document.documentElement`에 올바른 클래스가 적용되는지 테스트
    - 🟢 테마 토글 로직 구현
    - 🔵 전환 애니메이션, 깜빡임 방지 (SSR 초기값)
  - **예상 소요**: 2일
  - **의존성**: 없음

#### Phase 5-5: 지표 스트립

- [ ] `P5-008` 지표 데이터 수집 및 API
  - **설명**: 환율, 출산율 등 주요 지표 데이터를 수집하고 API로 제공한다
  - **영역**: 백엔드
  - **구현 사항**:
    - 지표 데이터 모델 (또는 기존 `product_info`/`product_prices` 테이블 활용)
    - `GET /api/v1/home/indicators` — 주요 지표 API
    - 스케줄러 잡: 지표 데이터 주기적 갱신
  - **테스트 (TDD)**:
    - 🔴 지표 API 응답 구조 검증 테스트
    - 🟢 데이터 수집 + API 구현
    - 🔵 캐싱 전략 적용 (지표는 빈번히 변하지 않음)
  - **예상 소요**: 2일
  - **의존성**: 없음
  - [PRD 확인 필요] 수집할 지표 종류와 데이터 소스 결정

- [ ] `P5-009` 지표 스트립 UI
  - **설명**: 메인 페이지 상단에 주요 지표를 스크롤/슬라이드 형태로 표시한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/home/ui/IndicatorStrip.tsx` — 지표 스트립 컴포넌트
    - 수치 변동 표시 (상승/하락 아이콘 + 색상)
  - **테스트 (TDD)**:
    - 🔴 지표 값 변동 방향에 따라 올바른 아이콘이 렌더링되는지 테스트
    - 🟢 UI 컴포넌트 구현
    - 🔵 애니메이션, 자동 슬라이드 추가
  - **예상 소요**: 1일
  - **의존성**: `P5-008`

#### Phase 5-6: 입법 현황 요약

- [ ] `P5-010` 입법 현황 데이터 수집 및 API
  - **설명**: 국회 입법 현황을 수집하고 요약하여 제공한다
  - **영역**: 백엔드
  - **구현 사항**:
    - 입법 현황 데이터 모델
    - 국회 공공 API 연동 또는 크롤링
    - `GET /api/v1/home/legislation` — 입법 현황 API
  - **테스트 (TDD)**:
    - 🔴 입법 현황 API 응답 구조 검증 테스트
    - 🟢 데이터 수집 + API 구현
    - 🔵 데이터 캐싱 및 갱신 주기 최적화
  - **예상 소요**: 3일
  - **의존성**: 없음
  - [PRD 확인 필요] 데이터 소스 및 범위 결정

- [ ] `P5-011` 입법 현황 UI
  - **설명**: 메인 페이지에 입법 현황 요약 섹션을 추가한다
  - **영역**: 프론트엔드
  - **구현 사항**:
    - `src/features/home/ui/LegislationSection.tsx` — 입법 현황 컴포넌트
  - **테스트 (TDD)**:
    - 🔴 입법 데이터가 올바르게 렌더링되는지 테스트
    - 🟢 UI 컴포넌트 구현
    - 🔵 접근성 개선
  - **예상 소요**: 1일
  - **의존성**: `P5-010`

#### 완료 기준
- 실시간 피드가 SSE로 동작한다
- 추적 중인 이슈에 업데이트 시 알림이 생성된다
- 게시글에 이미지를 첨부할 수 있다
- 다크 모드 전환이 동작한다
- 메인 페이지에 지표 스트립과 입법 현황이 표시된다
- 모든 테스트 통과

---

## Phase 6: 장기 확장

**목표**: AI 요약, 팔로우, 프리미엄, 오픈 API 등 서비스를 확장한다
**예상 기간**: 장기 (기능별 우선순위에 따라 진행)
**선행 조건**: Phase 3 이상
**TDD**: 필수

#### 태스크

- [ ] `P6-001` AI 이슈 자동 요약
  - **설명**: OpenAI API를 활용하여 이슈의 트리거 히스토리를 자동 요약한다
  - **영역**: 백엔드
  - **구현 사항**: 요약 생성 로직 + API 엔드포인트 + 스케줄러 잡
  - **테스트 (TDD)**: 요약 생성 로직 단위 테스트, API 통합 테스트
  - **예상 소요**: 3일
  - **의존성**: 없음

- [ ] `P6-002` 유저 팔로우/구독 시스템
  - **설명**: 사용자 간 팔로우 기능과 팔로우한 사용자의 활동 피드를 구현한다
  - **영역**: 백엔드 + 프론트엔드
  - **구현 사항**: 팔로우 모델 + API + UI
  - **테스트 (TDD)**: 팔로우/언팔로우 로직 테스트, 피드 정렬 테스트
  - **예상 소요**: 3일
  - **의존성**: 없음

- [ ] `P6-003` 프리미엄 멤버십
  - **설명**: 결제 시스템 연동 및 프리미엄 전용 기능 제한을 구현한다
  - **영역**: 백엔드 + 프론트엔드
  - **구현 사항**: 결제 모델 + PG 연동 + 권한 체계 확장
  - **테스트 (TDD)**: 결제 플로우 테스트, 권한 체크 테스트
  - **예상 소요**: 5일
  - **의존성**: 없음
  - [PRD 확인 필요] 프리미엄 전용 기능 범위, PG사 결정

- [ ] `P6-004` 오픈 API (공개 API)
  - **설명**: 외부 개발자가 사건/이슈 데이터를 활용할 수 있는 공개 API를 제공한다
  - **영역**: 백엔드
  - **구현 사항**: API 키 인증 + Rate Limiting + API 문서
  - **테스트 (TDD)**: API 키 인증 테스트, Rate Limiting 테스트
  - **예상 소요**: 3일
  - **의존성**: 없음

- [ ] `P6-005` 데이터 내보내기 (CSV/JSON)
  - **설명**: 사건/이슈 데이터를 CSV 또는 JSON 형식으로 다운로드할 수 있다
  - **영역**: 백엔드 + 프론트엔드
  - **구현 사항**: 내보내기 API + 다운로드 UI
  - **테스트 (TDD)**: CSV/JSON 직렬화 테스트
  - **예상 소요**: 2일
  - **의존성**: 없음

- [ ] `P6-006` 지표 상세 (시계열 그래프)
  - **설명**: 지표 데이터의 시계열 변화를 그래프로 시각화한다
  - **영역**: 프론트엔드
  - **구현 사항**: 차트 라이브러리 선정 + 시계열 그래프 UI
  - **테스트 (TDD)**: 데이터 변환 로직 테스트
  - **예상 소요**: 2일
  - **의존성**: `P5-008`

- [ ] `P6-007` 다국어 지원
  - **설명**: 한국어 외 영어 등 다국어 UI를 지원한다
  - **영역**: 프론트엔드
  - **구현 사항**: i18n 라이브러리 도입 + 번역 파일 + 언어 전환 UI
  - **테스트 (TDD)**: 언어 전환 시 올바른 텍스트 렌더링 테스트
  - **예상 소요**: 3일
  - **의존성**: 없음

#### 완료 기준
- 각 기능이 독립적으로 배포 가능한 상태이다
- 모든 기능에 테스트가 포함되어 있다

---

## 의존성 그래프

```
Phase 1 (골격)
  └── Phase 2 (어드민 UI)
        └── Phase 3 (어드민 연동) ──── MVP 완성
              ├── Phase 4 (어드민 고도화)
              │     ├── P4-1: 대시보드 (P4-001 → P4-002)
              │     ├── P4-2: 사용자 관리 (P4-003 → P4-004)
              │     ├── P4-3: 모더레이션 (P4-005 → P4-006)
              │     └── P4-4: 파이프라인 (P4-007 → P4-008)
              │
              └── Phase 5 (서비스 강화) ── Phase 4와 병렬 가능
                    ├── P5-1: 실시간 피드 (P5-001 → P5-002)
                    ├── P5-2: 알림 (P5-003 → P5-004)
                    ├── P5-3: 이미지 (P5-005 → P5-006)
                    ├── P5-4: 다크 모드 (P5-007, 독립)
                    ├── P5-5: 지표 (P5-008 → P5-009)
                    └── P5-6: 입법 (P5-010 → P5-011)

Phase 6 (확장) ── Phase 3 완료 후 언제든 시작 가능
  P6-001 ~ P6-007 (각각 독립적)
```

---

## 리스크 및 고려사항

| 리스크 | 영향도 | 대응 방안 |
|--------|--------|----------|
| 어드민 UI 구현 방식 미결정 (별도 SPA vs 기존 앱 내) | 중간 | 기존 Next.js 앱 내 `(admin)` 라우트 그룹으로 구현 (별도 SPA 대비 초기 비용 낮음) |
| 이미지 저장소 미결정 (S3 / R2) | 낮음 | 스토리지 어댑터 패턴으로 추상화하여 교체 가능하게 구현 |
| 신고 시스템 데이터 모델 미설계 | 중간 | Phase 4-3에서 설계, DB 마이그레이션 필요 (Alembic) |
| 프론트엔드 테스트 프레임워크 미설정 | 중간 | Phase 3 시작 전 Vitest + React Testing Library 도입 검토 |
| 배포 인프라 미결정 | 높음 | MVP 완성 후 Docker + VPS 또는 Cloud Run으로 결정 (Phase 4 이전) |
| CI/CD 파이프라인 미설정 | 중간 | Phase 4 시작 전 GitHub Actions 기반 CI 구축 권장 |
| SSE 실시간 피드 확장성 | 낮음 | 초기에는 SSE로 충분, 트래픽 증가 시 WebSocket 전환 검토 |
| 외부 API 의존성 (OpenAI, 국회 API) | 중간 | API 호출 실패 시 graceful degradation, 재시도 로직 구현 |

---

## PRD 추적성 매트릭스

| PRD 요구사항 | Spec ID | 태스크 ID | Phase | 상태 |
|-------------|---------|----------|-------|------|
| 메인 페이지 - 속보 피드 | S-M1-1, S-M1-2 | `DONE-001` | MVP | ✅ |
| 메인 페이지 - 타임라인 미니맵 | S-M2-1, S-M2-2 | `DONE-001` | MVP | ✅ |
| 메인 페이지 - 커뮤니티 핫 | S-M3-1 | `DONE-001` | MVP | ✅ |
| 메인 페이지 - 검색순위 | S-M4-1, S-M4-2 | `DONE-001` | MVP | ✅ |
| 메인 페이지 - 트렌드 | S-M5-1 | `DONE-001` | MVP | ✅ |
| 메인 페이지 - 주요 뉴스 | S-M6-1 | `DONE-001` | MVP | ✅ |
| 메인 페이지 - 커뮤니티 미디어 | S-M7-1 | `DONE-001` | MVP | ✅ |
| 타임라인 - 날짜 선택기 | S-T1-1, S-T1-2 | `DONE-002` | MVP | ✅ |
| 타임라인 - 필터/정렬 | S-T2-1, S-T2-2 | `DONE-002` | MVP | ✅ |
| 타임라인 - 사건 카드 | S-T4-1, S-T4-2 | `DONE-002` | MVP | ✅ |
| 타임라인 - 무한 스크롤 | S-T5-1 | `DONE-002` | MVP | ✅ |
| 타임라인 - 사건 상세 모달 | S-T6-1, S-T6-2 | `DONE-002` | MVP | ✅ |
| 이슈 추적 - 목록/필터/정렬 | S-I1-1, S-I1-2 | `DONE-003` | MVP | ✅ |
| 이슈 추적 - 이슈 카드 | S-I3-1, S-I3-2 | `DONE-003` | MVP | ✅ |
| 이슈 추적 - 이슈 상세 | S-I4-1 ~ S-I4-4 | `DONE-003` | MVP | ✅ |
| 이슈 추적 - 추적 등록/해제 | S-I5-1 ~ S-I5-3 | `DONE-003` | MVP | ✅ |
| 커뮤니티 - 탭/정렬 | S-C1-1, S-C1-2 | `DONE-004` | MVP | ✅ |
| 커뮤니티 - 글쓰기 | S-C3-1 ~ S-C3-3 | `DONE-004` | MVP | ✅ |
| 커뮤니티 - 게시글 상세/댓글 | S-C4-1 ~ S-C4-7 | `DONE-004` | MVP | ✅ |
| 검색 - 통합 검색 | S-S1-1 ~ S-S5-1 | `DONE-005` | MVP | ✅ |
| 내 추적 - 모아보기 | S-TR1-1 ~ S-TR1-5 | `DONE-006` | MVP | ✅ |
| MY 페이지 - 프로필/계정 | S-MY1-1 ~ S-MY4-2 | `DONE-007` | MVP | ✅ |
| 인증 | — | `DONE-008` | MVP | ✅ |
| 어드민 - 사건/이슈/트리거 CRUD | S-AD2-1 ~ S-AD2-9 | `P1-001` ~ `P3-003` | 1~3 | ⬜ |
| 어드민 - 태그/출처 CRUD | S-AD3-1 ~ S-AD3-5 | `P1-001` ~ `P3-004` | 1~3 | ⬜ |
| 어드민 - 대시보드 | S-AD1-1 ~ S-AD1-4 | `P4-001`, `P4-002` | 4 | ⬜ |
| 어드민 - 사용자 관리 | S-AD4-1 ~ S-AD4-3 | `P4-003`, `P4-004` | 4 | ⬜ |
| 어드민 - 모더레이션 | S-AD5-1 ~ S-AD5-3 | `P4-005`, `P4-006` | 4 | ⬜ |
| 어드민 - 파이프라인 모니터링 | S-AD6-1 ~ S-AD6-4 | `P4-007`, `P4-008` | 4 | ⬜ |
| 실시간 피드 | S-F1-1 | `P5-001`, `P5-002` | 5 | ⬜ |
| 알림 시스템 | — | `P5-003`, `P5-004` | 5 | ⬜ |
| 이미지 첨부 | — | `P5-005`, `P5-006` | 5 | ⬜ |
| 다크 모드 | — | `P5-007` | 5 | ⬜ |
| 지표 스트립 | — | `P5-008`, `P5-009` | 5 | ⬜ |
| 입법 현황 | — | `P5-010`, `P5-011` | 5 | ⬜ |
| AI 이슈 요약 | — | `P6-001` | 6 | ⬜ |
| 유저 팔로우 | — | `P6-002` | 6 | ⬜ |
| 프리미엄 멤버십 | — | `P6-003` | 6 | ⬜ |
| 오픈 API | — | `P6-004` | 6 | ⬜ |
| 데이터 내보내기 | — | `P6-005` | 6 | ⬜ |
| 지표 상세 | — | `P6-006` | 6 | ⬜ |
| 다국어 지원 | — | `P6-007` | 6 | ⬜ |
| 뉴스 수집 파이프라인 | — | `DONE-010` | MVP | ✅ |
| 스케줄러 워커 | — | `DONE-011` | MVP | ✅ |
| SEO (SSR, 메타태그, sitemap) | — | — | — | [PRD 확인 필요] 별도 태스크 필요 여부 |
| 성능 목표 (FCP, LCP, API P95) | — | — | — | Phase 4 이후 성능 측정/최적화 |
| 접근성 (WCAG 2.1 AA) | — | — | — | 전 Phase에 걸쳐 점진적 적용 |
| Rate Limiting | — | — | — | [PRD 확인 필요] 적용 시점 결정 |
