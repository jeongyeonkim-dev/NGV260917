# 서브에이전트·스킬 경계 지도

이 프로젝트의 `.claude/agents/`와 `.claude/skills/`에 있는 모든 서브에이전트·스킬의 담당 범위와, 서로 겹치지 않도록 정한 경계를 정리한 문서다. 새 에이전트/스킬을 추가하거나 기존 것을 고칠 때는 이 표를 먼저 확인해서 중복 정의를 만들지 않는다.

## 1. 개발 생명주기 파이프라인 (분석 → 설계 → 구현 → 테스트)

| 순서 | 단계 | A-SPICE 프로세스 | 서브에이전트 | 스킬 | 테스트/설계 베이시스 |
|------|------|-------------------|---------------|------|------------------------|
| 1 | 요구사항 분석 | SYS.1/SYS.2/SWE.1 | `requirements-analyst` | `requirements-analyst` | 이해관계자 요구/상위 요구사항 |
| 2 | 아키텍처 설계 | SYS.3/SWE.2 (+SYS.4/SWE.5 통합순서) | `architecture-designer` | `architecture-designer` | 요구사항(1단계 산출물) |
| 3 | 상세설계 | SWE.3 | `detailed-design` | `detailed-design` | 아키텍처(2단계 산출물) |
| 4 | 구현 + 단위테스트 | SWE.3(구현)/SWE.4(단위검증) | `coding` | `coding`(표준·품질지표) + `tdd`(Red-Green-Refactor 절차) | 상세설계(3단계 산출물) |
| 5 | 컴포넌트 통합시험 | SWE.5 | `integration-tester` | `integration-testing` | **아키텍처의 컴포넌트 인터페이스 명세 + 통합 순서**(2단계 산출물) |
| 6 | 시스템(소프트웨어) 검증 | SWE.6 | `sw-system-tester` | `sw-system-test` | **SW 요구사항 명세서**(1단계 산출물) |

파이프라인과 별도로:

| 역할 | 성격 | 서브에이전트 | 스킬 | 비고 |
|------|------|---------------|------|------|
| CL2 준수 감사 | 사후 점검(생산이 아니라 감사) | `aspice-cl2-auditor` | `aspice-auditor` | 위 1~6단계가 만든 산출물이 A-SPICE 4.1 CL2(PA2.1/PA2.2)를 충족하는지 나중에 점검. 산출물을 새로 만들지 않는다. |

## 2. 소프트웨어 테스트 3단계의 명확한 구분 (가장 헷갈리기 쉬운 경계)

| 구분 | SWE.4 단위테스트 | SWE.5 통합시험 | SWE.6 시스템 검증 |
|------|--------------------|-------------------|----------------------|
| 담당 | `coding`+`tdd` | `integration-tester`+`integration-testing` | `sw-system-tester`+`sw-system-test` |
| 테스트 베이시스 | 상세설계 단위 인터페이스 | 아키텍처 컴포넌트 인터페이스 + 통합순서 | SW 요구사항 명세서 |
| 관점 | 화이트박스(코드 내부) | 화이트박스(컴포넌트 경계) | 블랙박스(요구사항 대비 결과) |
| 커버리지 지표 | 구문/분기(향후 필요 시 추가) | **함수 커버리지·Call 커버리지 100%** | 요구사항 커버리지(모든 SW-REQ가 케이스로 다뤄졌는지) |
| 산출물 형태 | Python 테스트 코드(unittest) | Python 테스트 코드(unittest) | 테스트 케이스 시트(xlsx) |

세 스킬 모두 아래 두 가지를 **공통으로 재사용**하고, 각자 다시 정의하지 않는다:
- **핵심 테스트 설계 기법(경계값분석/동등분할/오류추측)의 정의** — 원본은 `tdd/references/test-case-design.md`. `integration-testing`은 그대로 재사용하고 ISO26262-6 통합시험기법(인터페이스/결함주입/자원사용/백투백)만 추가. `sw-system-test`도 그대로 재사용하고 조합/페어와이즈(PICT)만 추가.
- **긍정(Positive)/부정(Negative) 케이스 분류** — 모든 테스트 산출물(코드 테스트 함수든 xlsx 시트든)에 필수 표기. 코드 테스트는 `tdd/references/test-doxygen-convention.md`의 `@testtype` 태그로, 시스템 테스트 케이스는 xlsx의 "긍정/부정" 컬럼으로 표기.

## 3. 교차 관심사의 단일 소유권 (중복 정의 금지)

아래 개념은 프로젝트 전체에서 **한 곳에서만 정의**하고 나머지는 참조만 한다.

| 개념 | 정의를 소유하는 곳 | 참조하는 곳 |
|------|----------------------|---------------|
| 요구사항/아키텍처/설계/코드 ID 채번 규칙, 추적매트릭스 위치 | `requirements-analyst/references/traceability-scheme.md` | `detailed-design/references/traceability-continuation.md`(단위·코드 레벨로 확장), `integration-testing`, `sw-system-test`(TC-SYS-#### 채번만 자체 정의, 나머지는 재사용) |
| ISO/IEC 25010 품질특성(NFR 분류 체계) | `requirements-analyst/references/iso25010-nfr-checklist.md`(2023년판, 9특성) | `sw-system-test`(비기능 테스트 케이스 시트 분류 기준) |
| 테스트 설계 기법(경계값분석/동등분할/오류추측/상태기반/회귀재현) | `tdd/references/test-case-design.md` | `integration-testing`, `sw-system-test` |
| 테스트 함수 Doxygen 목적 설명 규칙(`@brief`/`@testtype`/`@technique`) | `tdd/references/test-doxygen-convention.md` | `integration-testing/references/integration-test-doxygen-extension.md`(태그 추가만) |
| 인터페이스 전용 통신·인터페이스 명세 필수 항목 | `architecture-designer/references/interface-first-design.md` | `detailed-design/references/unit-interface-and-datarange.md`(단위 수준으로 확장) |
| A-SPICE 4.1 프로세스별 원문 근거(BP/출력산출물) | 각 스킬의 `references/aspice-*-bp.md`(프로세스마다 그 프로세스를 담당하는 스킬이 소유) | `aspice-auditor/references/process-work-products.md`(전체 프로세스의 산출물 코드만 요약 색인 — 상세 BP 설명은 재수록하지 않고 각 스킬을 가리킴) |
| ISO 26262-6 소프트웨어 설계원칙(응집도/결합도, 안전코딩원칙) | 아키텍처 수준: `architecture-designer/references/iso26262-6-design-principles.md` / 단위 수준: `detailed-design/references/iso26262-6-unit-design-principles.md` | `coding/references/aspice-iso26262-coding-link.md`(Python 적용만 추가, 원칙 재정의 없음) |

## 4. 새 에이전트/스킬을 추가할 때 지킬 규칙
1. 이미 다른 스킬이 소유한 개념(위 3번 표)은 재정의하지 말고 상대경로 참조 문구(`"<스킬명>"의 references/<파일>.md 참조`)로 연결한다.
2. 각 스킬의 `SKILL.md`에는 "이 스킬의 경계(다루지 않는 것)" 절을 두어, 인접 단계와의 경계를 명시한다.
3. 이 파일(`PIPELINE.md`)에 새 행을 추가해 갱신한다 — 개별 스킬 파일만 고치고 이 지도를 갱신하지 않으면 다음에 또 같은 혼선이 생긴다.
