---
name: writing-prd
description: PRD 작성 가이드. PRD 작성, 기획, 제품 개발 시작, AI 에이전트에 개발 요청 시 사용. "PRD 작성", "기획서", "제품 요구사항", "스펙 작성" 등의 요청에 트리거.
---

<Purpose>
AI 에이전트(Claude Code 등)와 함께 제품을 개발하는 솔로 개발자를 위한 PRD 작성 방법론.
핵심: PRD는 "형상 공유 문서(Visualization Sync)"이자 "Single Source of Truth"다.
잘 쓴 PRD = AI 에이전트가 정확히 구현할 수 있는 스펙.
</Purpose>

<Use_When>
- 새 제품/기능의 PRD를 작성할 때
- AI 에이전트에 개발을 요청하기 전 스펙을 정리할 때
- 기존 PRD를 개선하거나 리뷰할 때
- "무엇을 만들지"를 구조화해야 할 때
</Use_When>

<Do_Not_Use_When>
- 단순 버그 수정 (이슈 트래커로 충분)
- 이미 PRD가 있고 수정 없이 구현만 하는 경우
- 기술적 리팩토링 (ADR이 더 적합)
</Do_Not_Use_When>

<Core_Pattern>
## WHY-HOW-WHAT 프레임워크

모든 PRD는 이 순서로 작성한다:
1. **WHY** (배경/맥락): 왜 이 제품이 필요한가? 어떤 문제를 해결하는가?
2. **HOW** (접근 방식): 어떤 방식으로 문제를 해결할 것인가?
3. **WHAT** (구체적 요구사항): 정확히 무엇을 만들 것인가?

## 계층적 구조화 (Requirement → Feature → Spec → Userflow)

PRD를 AI가 읽고 코드와 대조할 수 있으려면, 자유 텍스트가 아닌 **계층화된 데이터**로 작성해야 한다.

```
Requirement (요구사항)
  └─ Feature (기능)
       └─ Spec (명세)
            └─ Userflow (사용자 흐름)
```

- **Requirement**: 비즈니스/사용자 요구 (예: "리셀러 정산 시스템")
- **Feature**: 구현 단위 기능 (예: "할인 코드 발급", "커미션 정산")
- **Spec**: 구체적 동작 명세 (예: "할인 코드 생성 시 Paddle API 호출 필수")
- **Userflow**: 사용자 관점 흐름 (예: "리셀러가 코드 생성 → 유저가 코드 입력 → 할인 적용 확인")

이 구조가 있으면 AI가 각 Spec이 코드에 구현되었는지 자동 대조(GAP 분석)할 수 있다.

## PRD 필수 구성 요소

| 우선순위 | 섹션 | 설명 |
|---------|------|------|
| **필수** | Persona | 구체적 사용자 프로필 (이름, 맥락, 니즈, 숙련도) |
| **필수** | Main Scenario | 핵심 사용 시나리오 (사용자 관점으로 서술) |
| **필수** | Feature-Spec 계층 | Requirement→Feature→Spec→Userflow 구조화 |
| 기본 | Background & Context | 문제 정의, 시장 맥락 |
| 기본 | Value Definition | 이 제품이 제공하는 핵심 가치 |
| 심화 | Use Cases | 상세 유즈케이스 (정상/예외/엣지) |
| 심화 | User Journey | 사용자 여정 플로우 |
| 심화 | Success Criteria | 측정 가능한 성공 지표 |

**핵심 원칙**: 요구사항의 주어는 항상 "사용자"다.
- X: "시스템이 알림을 보낸다"
- O: "사용자가 알림을 받는다"
</Core_Pattern>

<Completeness_Model>
## 30점 모델 (반복적 완성)

PRD는 완벽할 필요 없다. 30%에서 시작하여 반복한다.

```
[30점] 초안 작성 → Persona + Main Scenario만으로 시작
  ↓ 피드백 & AI 에이전트 협업
[60점] 유즈케이스, 경계 조건 추가
  ↓ 프로토타입 & 검증
[100점] 배포 가능한 완성 스펙
  ↓ 출시 후 새로운 요구사항 발생
[70점] 다시 순환 → 다음 버전 PRD
```

- 30점 초안 단계에서 AI 에이전트에 피드백을 요청하면 빠르게 개선 가능
- 와이어프레임, 도식, 표를 활용하면 가독성이 크게 향상됨
</Completeness_Model>

<AI_Agent_Guide>
## AI 에이전트 활용 가이드

### 모듈러 프롬프트 원칙
- PRD 전체를 한번에 넣지 말고, **시나리오별로 하나씩** 점진적으로 개발
- 각 시나리오가 동작하면 다음 시나리오로 확장

### Spec-Driven Development
1. **Specify**: PRD 작성 (이 스킬 활용)
2. **Plan**: 구현 계획 수립 (AI 에이전트와 협업)
3. **Tasks**: 작업 단위로 분해
4. **Implement**: 시나리오별 점진적 구현

### 3-Tier 경계 시스템
PRD에 AI 에이전트의 행동 경계를 명시한다:
- **Always**: 항상 지켜야 할 규칙 (코드 스타일, 테스트 필수 등)
- **Ask First**: 판단이 필요한 영역 (DB 스키마 변경, 외부 API 추가 등)
- **Never**: 절대 하지 말아야 할 것 (데이터 삭제, 프로덕션 직접 접근 등)

### AI 에이전트용 6대 명시 영역
1. **Commands**: 빌드, 테스트, 린트 명령어
2. **Testing**: 테스트 전략과 커버리지 기준
3. **Project Structure**: 디렉토리 구조와 파일 명명 규칙
4. **Code Style**: 코딩 컨벤션과 패턴
5. **Git Workflow**: 브랜치, 커밋, PR 규칙
6. **Boundaries**: 위 3-Tier 경계 시스템
</AI_Agent_Guide>

<Quick_Checklist>
## PRD 작성 체크리스트

- [ ] WHY가 명확한가? (문제 정의)
- [ ] Persona가 구체적인가? (이름, 맥락, 니즈)
- [ ] Main Scenario가 사용자 관점으로 서술되었는가?
- [ ] Feature-Spec 계층이 구조화되었는가? (Requirement→Feature→Spec→Userflow)
- [ ] 외부 서비스 연동이 Spec에 명시되었는가? (결제, 인증, 알림 등)
- [ ] 성공 기준이 측정 가능한가?
- [ ] AI 에이전트용 경계(Always/Ask/Never)가 정의되었는가?
- [ ] 기술 스택과 제약 조건이 명시되었는가?
- [ ] **AI 가독성 테스트 통과**: PRD를 AI에 입력하고 "Feature 목록 찾아줘"라고 했을 때 정확한 목록이 나오는가?
</Quick_Checklist>

<Common_Mistakes>
## 흔한 실수

1. **시스템 관점 서술**: "서버가 데이터를 저장한다" → "사용자가 데이터를 저장할 수 있다"
2. **모호한 요구사항**: "빠르게 로딩" → "첫 페이지 로드 2초 이내"
3. **한번에 완벽하게**: 30점 초안으로 시작해서 반복하라
4. **AI에 전체 덤프**: 시나리오별로 나누어 점진적으로 전달하라
5. **경계 미설정**: Always/Ask/Never를 명시하지 않으면 AI가 임의로 판단한다
6. **비정형 텍스트 기획서**: 노션에 자유롭게 쓴 텍스트는 AI가 구조를 파악 못한다. Requirement→Feature→Spec 계층으로 구조화하라
7. **외부 연동 누락**: "Paddle 연동"이라고 썼지만 어떤 Spec의 어떤 단계에서 호출하는지 빠뜨리면, AI가 DB 저장만 구현하고 API 호출을 누락한다
8. **시각화 없는 텍스트만의 기획**: 와이어프레임, 플로우차트 없이 텍스트만 쓰면 모호함이 커진다. 문서화 + 시각화를 병행하라
</Common_Mistakes>

<Template>
PRD 작성 시 `prd-template.md` 템플릿을 참조하라.
위치: 이 스킬과 같은 디렉토리의 `prd-template.md`
</Template>
