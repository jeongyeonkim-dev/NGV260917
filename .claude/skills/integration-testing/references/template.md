# 통합시험 산출물 양식 (확정 — WP_Templates 기준)

> 출처: `WP_Templates/Engineering/SoftwareComponentVerificationAndIntegrationVerification/`의 `TPL-SWE5-001`(SW 통합전략 및 통합시험 명세서, docx), `TPL-SWE5-002`(SW 통합시험 케이스, xlsx), `TPL-SWE5-003`(SW 통합시험 결과서, xlsx). 저작권: Synetics — 교육 과정 밖 배포·공개 금지 고지를 문서 통제 절에 유지한다.

이 파일은 더 이상 임시(INTERIM) 상태가 아니다.

## TPL-SWE5-001 (통합전략 및 통합시험 명세서, docx) 목차
0. 문서 통제
1. 목적 및 적용범위
2. 통합 원칙 (통합단위/단계/위험우선순위/반복가능성/실패격리)
3. 통합 항목과 순서 (ID/선행조건/의존성/순서/담당/계획 베이스라인 — `integration-sequence.md` 산출물을 그대로 옮김)
4. 환경 및 형상 (도구/실행환경/SW버전/시험데이터/형상식별)
5. 진입 및 종료 기준 (각 통합단계의 시작/중단/재개/완료 판정 기준)
6. 통합시험 케이스 요약 (ID/추적대상/기대결과/자동화여부 — 상세는 TPL-SWE5-002에)
7. 시험 설계기법 (경계값/동등분할/의사결정표/상태전이 등 선정 근거 — `iso26262-6-integration-test-methods.md`)
8. 실행 및 결과 기록 규칙
9. 회귀 전략
10. 실패 및 편차 처리
11. 추적성과 보고
12. 적용 한계 (통합시험으로 확인 못하는 시스템/HIL/차량/양산 범위)
13. 추적성
14. 참고자료

## TPL-SWE5-002 (통합시험 케이스, xlsx) — 시트 "Integration Cases"
고정 컬럼: **Test ID | Trace | Integration Item | Stimulus | Expected Result | Technique | Automation**

- "Trace" 컬럼에 `requirements-analyst`의 `SWR-###` 및 아키텍처 인터페이스 ID를 함께 적는다(예: `SWR-007 / IF-CompA-CompB`).
- "Technique" 컬럼에 `tdd`의 `test-case-design.md` 공통기법 + 이 스킬 고유의 ISO26262-6 기법(요구사항기반/인터페이스/결함주입/자원사용/백투백) 중 사용한 것을 적는다.
- **긍정/부정 구분**: 기본 템플릿에는 별도 컬럼이 없다 — 이 프로젝트는 "Technique" 뒤에 `(긍정)`/`(부정)`을 괄호로 병기하거나, Test ID 접두사(`TC-I-POS-###`/`TC-I-NEG-###`)로 구분한다(둘 중 하나로 통일, 처음 정한 방식을 계속 유지).
- "Integration Item"에는 `integration-sequence.md`의 단계 번호와 통합 대상 컴포넌트를 적는다(`integration-test-doxygen-extension.md`의 `@integrationstep`/`@interface`와 동일 정보).

## TPL-SWE5-003 (통합시험 결과서, xlsx)
실행 결과(Pass/Fail), 실행일시, 환경, 실제결과, 증거 위치, 결함 연결을 기록한다 — `coverage-100-percent-strategy.md`의 함수/Call 커버리지 실측 결과도 이 결과서에 포함한다.

## 필드 매핑
- 이 스킬 SKILL.md의 출력 형식(통합시험 개요/단계별 계획/시험케이스목록/커버리지결과/추적성)은 TPL-SWE5-001(개요·계획·기법·추적) + TPL-SWE5-002(케이스 목록) + TPL-SWE5-003(결과·커버리지)의 3분할 구조에 대응한다.
