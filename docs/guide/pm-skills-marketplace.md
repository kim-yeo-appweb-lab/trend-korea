# PM Skills Marketplace 사용 가이드

> 65개 PM 스킬 + 36개 체인 워크플로우 | 8개 Claude 플러그인 | 검증된 PM 프레임워크 기반

## 1. 개요

PM Skills Marketplace는 제품 관리(PM) 프레임워크를 Claude AI에 내장하여, 단순 텍스트 생성이 아닌 **구조화된 의사결정**을 지원하는 오픈소스 스킬 모음입니다.

Teresa Torres, Marty Cagan, Alberto Savoia 등의 방법론이 일상 워크플로우에 녹아들어 있습니다.

---

## 2. 핵심 개념

| 개념 | 설명 | 사용 방법 |
|------|------|-----------|
| **스킬(Skill)** | 특정 PM 작업을 위한 프레임워크/워크플로우 | 대화 중 자동 로드 |
| **커맨드(Command)** | 여러 스킬을 체인으로 연결한 워크플로우 | `/커맨드명`으로 실행 |
| **플러그인(Plugin)** | 관련 스킬/커맨드를 묶은 설치 패키지 | 도메인별 설치 |

> **핵심**: 커맨드는 `/`가 필요하고, 스킬은 대화 맥락에 맞게 자동 로드됩니다.

---

## 3. 설치

### 3.1 Claude Code (CLI)

```bash
# 마켓플레이스 추가
claude plugin marketplace add phuryn/pm-skills

# 플러그인 개별 설치
claude plugin install pm-toolkit@pm-skills
claude plugin install pm-product-strategy@pm-skills
claude plugin install pm-product-discovery@pm-skills
claude plugin install pm-market-research@pm-skills
claude plugin install pm-data-analytics@pm-skills
claude plugin install pm-marketing-growth@pm-skills
claude plugin install pm-go-to-market@pm-skills
claude plugin install pm-execution@pm-skills
```

### 3.2 Claude Cowork (비개발자 권장)

1. 좌측 하단 **Customize** 클릭
2. **Browse plugins > Personal > +** 이동
3. **Add marketplace from GitHub** 선택
4. `phuryn/pm-skills` 입력

### 3.3 기타 AI 도구 (스킬만 사용)

```bash
# 예: Gemini CLI용 스킬 복사
for plugin in pm-*/; do
  cp -r "$plugin/skills/"* ~/.gemini/skills/ 2>/dev/null
done
```

---

## 4. 플러그인별 가이드

### 4.1 pm-product-discovery — 제품 발견

지속적 제품 발견: 아이디어 도출, 실험 설계, 가정 검증, 기능 우선순위, OST, 고객 인터뷰

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/discover` | 전체 발견 사이클 (아이디어 > 가정 > 우선순위 > 실험) | `/discover 원격 팀을 위한 AI 회의 요약 도구` |
| `/brainstorm` | 다관점 아이디어 도출 | `/brainstorm experiments existing — 온보딩 이탈 줄이기` |
| `/triage-requests` | 기능 요청 분석 및 우선순위 지정 | `/triage-requests [CSV 첨부]` |
| `/interview` | 인터뷰 스크립트 준비 또는 요약 | `/interview prep — 구매 담당자 인터뷰` |
| `/setup-metrics` | 제품 메트릭 대시보드 설계 | `/setup-metrics` |

**스킬 활용 예시:**
- "우리 AI 작문 도우미 아이디어에서 가장 위험한 가정은?"
- "사용자 활성화 개선을 위한 Opportunity Solution Tree를 만들어줘"

---

### 4.2 pm-product-strategy — 제품 전략

제품 전략, 비전, 비즈니스 모델, 가격 정책, 거시 환경 분석

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/strategy` | 9섹션 제품 전략 캔버스 | `/strategy 에이전시용 B2B 프로젝트 관리 도구` |
| `/business-model` | 비즈니스 모델 탐색 | `/business-model startup — AI 작문 도구` |
| `/value-proposition` | JTBD 기반 가치 제안 설계 | `/value-proposition 기업용 SaaS 온보딩 도구` |
| `/market-scan` | 거시 환경 분석 (SWOT+PESTLE+Porter+Ansoff) | `/market-scan` |
| `/pricing` | 가격 전략 및 경쟁 분석 | `/pricing` |

---

### 4.3 pm-execution — 실행

PRD, OKR, 로드맵, 스프린트, 회고, 릴리스 노트, 사전 부검, 이해관계자 관리

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/write-prd` | PRD 작성 | `/write-prd 알림 피로를 줄이는 스마트 알림 시스템` |
| `/plan-okrs` | 팀 OKR 브레인스토밍 | `/plan-okrs` |
| `/sprint` | 스프린트 라이프사이클 (plan/retro/release) | `/sprint retro — 지난 스프린트 노트` |
| `/write-stories` | 기능을 백로그 아이템으로 분해 | `/write-stories job — 팀 대시보드 기능` |
| `/pre-mortem` | 사전 부검 리스크 분석 | `/pre-mortem` |
| `/test-scenarios` | 유저 스토리 기반 테스트 시나리오 | `/test-scenarios` |
| `/generate-data` | 테스트용 더미 데이터 생성 | `/generate-data` |

---

### 4.4 pm-market-research — 시장 조사

사용자 리서치, 경쟁 분석: 페르소나, 세그먼테이션, 여정 맵, 시장 규모 추정

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/research-users` | 페르소나 구축 + 세그먼트 + 여정 맵 | `/research-users 피트니스 앱 사용자 12명 인터뷰 데이터` |
| `/competitive-analysis` | 경쟁 환경 분석 | `/competitive-analysis 디자인 도구 시장의 Figma 경쟁자` |
| `/analyze-feedback` | 사용자 피드백 감성 분석 | `/analyze-feedback Q4 NPS 응답 200건 [파일 첨부]` |

---

### 4.5 pm-data-analytics — 데이터 분석

SQL 쿼리 생성, 코호트 분석, A/B 테스트 분석

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/write-query` | 자연어 → SQL 쿼리 생성 | `/write-query Q4 국가별 월간 활성 사용자 (BigQuery)` |
| `/analyze-cohorts` | 코호트별 리텐션 분석 | `/analyze-cohorts 1월 vs 2월 가입자 주간 리텐션` |
| `/analyze-test` | A/B 테스트 결과 분석 | `/analyze-test 결제 플로우 A/B 테스트 결과 [CSV 첨부]` |

---

### 4.6 pm-go-to-market — GTM 전략

비치헤드 세그먼트, ICP, 메시징, 성장 루프, GTM 모션, 경쟁 배틀카드

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/plan-launch` | 전체 GTM 전략 수립 | `/plan-launch 중규모 엔지니어링 팀용 AI 코드 리뷰 도구` |
| `/growth-strategy` | 성장 루프 + GTM 모션 설계 | `/growth-strategy 프리랜서-스타트업 양면 마켓플레이스` |
| `/battlecard` | 경쟁 배틀카드 작성 | `/battlecard 우리 CRM vs Salesforce (SMB 시장)` |

---

### 4.7 pm-marketing-growth — 마케팅 & 성장

마케팅 아이디어, 포지셔닝, 가치 제안 문구, 제품 네이밍, North Star 메트릭

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/market-product` | 마케팅 아이디어 + 포지셔닝 + 네이밍 | `/market-product 이커머스 관리자용 B2B 분석 대시보드` |
| `/north-star` | North Star 메트릭 정의 | `/north-star 프리랜서-클라이언트 양면 마켓플레이스` |

---

### 4.8 pm-toolkit — PM 유틸리티

이력서 리뷰, 법률 문서, 교정

**주요 커맨드:**

| 커맨드 | 설명 | 예시 |
|--------|------|------|
| `/review-resume` | PM 이력서 리뷰 | `/review-resume [PDF 첨부]` |
| `/tailor-resume` | 채용 공고에 맞춘 이력서 조정 | `/tailor-resume [이력서 + 채용 공고 첨부]` |
| `/draft-nda` | NDA 초안 작성 | `/draft-nda` |
| `/privacy-policy` | 개인정보 처리방침 초안 | `/privacy-policy` |
| `/proofread` | 문법, 논리, 흐름 검토 | `/proofread Q1 투자자 업데이트 초안` |

---

## 5. 워크플로우 체이닝

커맨드는 PM 워크플로우 순서에 맞게 설계되어 있습니다. 각 커맨드 완료 후 다음 추천 커맨드가 제안됩니다.

### 추천 워크플로우 예시

```
제품 발견 → 전략 수립 → 실행
─────────────────────────────
/discover          → 아이디어 + 가정 검증
  ↓
/strategy          → 제품 전략 캔버스
  ↓
/business-model    → 비즈니스 모델 설계
  ↓
/write-prd         → PRD 작성
  ↓
/write-stories     → 백로그 아이템 분해
  ↓
/sprint plan       → 스프린트 계획
```

### GTM 워크플로우 예시

```
시장 조사 → GTM → 마케팅
─────────────────────────
/research-users         → 페르소나 + 여정 맵
  ↓
/competitive-analysis   → 경쟁 분석
  ↓
/plan-launch            → GTM 전략
  ↓
/market-product         → 마케팅 + 포지셔닝
  ↓
/north-star             → 핵심 메트릭 정의
```

---

## 6. 팁

- **스킬은 자동 로드됩니다** — 관련 대화를 하면 Claude가 알아서 적용합니다
- **강제 로드**: `/플러그인명:스킬명` (예: `/pm-product-strategy:swot-analysis`)
- **데이터 첨부**: CSV, PDF 파일을 첨부하면 분석 스킬이 더 정확해집니다
- **커맨드 연계**: 완료 후 제안되는 다음 커맨드를 따라가면 자연스러운 워크플로우가 됩니다

---

## 참고

- GitHub: [phuryn/pm-skills](https://github.com/phuryn/pm-skills)
- 기반 프레임워크: Teresa Torres (CDH), Marty Cagan (INSPIRED), Alberto Savoia (The Right It), Dan Olsen (Lean Product Playbook) 등
- 출처: [The Product Compass](https://www.theproductcompass.pm/) by Pawel Huryn
