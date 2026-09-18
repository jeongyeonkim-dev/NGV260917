# 시스템 테스트 산출물 양식 (확정 — WP_Templates 기준)

> 출처: `WP_Templates/Engineering/SoftwareVerification/`의 `TPL-SWE6-001`(SW 검증 명세서, xlsx: 시트 "Verification Specification"/"Environment"/"Change History"), `TPL-SWE6-002`(SW 검증 결과서, xlsx). 저작권: Synetics — 교육 과정 밖 배포·공개 금지 고지를 유지한다.

## TPL-SWE6-001 — 시트 "Verification Specification" (고정 컬럼)
**Test ID | SW Req | Level/Environment | Stimulus | Expected Result | Technique | Execution**

이 스킬(`sw-system-test`)의 SKILL.md "산출물 구조" 절이 정의한 필드는 이 고정 컬럼에 아래와 같이 매핑한다 — 컬럼 자체를 늘리지 않고 기존 컬럼 안에서 표현한다:

| 이 스킬의 필드 | TPL-SWE6-001 컬럼 | 표기 방법 |
|-----------------|----------------------|------------|
| 테스트 케이스 ID | `Test ID` | `TC-SYS-####` |
| 대상 요구사항 | `SW Req` | `SWR-###`(`requirements-analyst`의 확정 ID, `traceability-scheme.md` 참조) |
| 사전조건 + 테스트 절차 + 입력값 | `Stimulus` | "사전조건: ... / 절차: 1) ... 2) ... / 입력: ..." 형식으로 한 셀에 순서대로 기술 |
| 예상 결과 | `Expected Result` | 그대로 |
| 사용 기법 | `Technique` | `tdd`의 공통기법 + 이 스킬 고유 기법(조합/페어와이즈) |
| **긍정/부정 구분**(이 프로젝트 필수 규칙) | `Technique` 뒤에 병기 | 예: `경계값분석(긍정)`, `오류추측(부정)` — 또는 `Test ID`를 `TC-SYS-POS-###`/`TC-SYS-NEG-###`로 구분(택1, 한 번 정하면 계속 유지) |
| 우선순위 | 별도 컬럼 없음 | `Test ID` 채번 순서로 우선순위를 대체하거나, 조직이 허용하면 컬럼을 1개 추가한다(임의 추가 전 사용자 확인) |
| 확정 근거(원문에 없는 판정기준 확정 시) | 별도 컬럼 없음 | 조직이 허용하면 컬럼 추가, 아니면 "Verification Specification" 시트 하단에 각주로 기록 |
| 실행 환경 | `Level/Environment` | "시스템(PC/SIL)" 등 |

## TPL-SWE6-001 — 시트 "Environment"
컬럼: Environment ID | 구성 | 버전 및 식별 | 사용 범위 | 수집 증거 | 명시적 제외. OEM 문서 3절(실행환경: Python PC/SIL, Web 시뮬레이터)을 이 시트에 반영한다.

## TPL-SWE6-002 — SW 검증 결과서
`Execution`(TPL-SWE6-001) 결과와 연결해 Pass/Fail, 실행일시, 증거 위치, 결함 연결을 기록한다. **시험 성공률 100%**(CLAUDE.md 근거) 여부를 이 결과서에서 집계한다.

## 비기능 테스트 케이스
비기능(NFR) 케이스도 같은 컬럼 구조를 쓰되, `SW Req`에 `SWR-###-<ISO25010특성태그>`(예: `SWR-016-PERF`)를 적어 특성별로 필터링 가능하게 한다. 별도 시트로 분리하고 싶으면 "Verification Specification"을 복제해 "NFR Verification Specification" 시트를 추가한다(원본 구조를 해치지 않는 선에서 조직에 확인 후 진행).
