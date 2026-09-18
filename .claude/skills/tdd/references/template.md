# 단위시험 산출물 양식 (확정 — WP_Templates 기준)

> 출처: `WP_Templates/Engineering/SoftwareUnitVerification/`의 `TPL-SWE4-001`(SW 단위시험 명세서, xlsx: 시트 "Unit Test Specification"/"Entry Exit Criteria"/"Change History"), `TPL-SWE4-002`(SW 단위시험 결과서, xlsx). 저작권: Synetics — 교육 과정 밖 배포·공개 금지 고지를 유지한다.

이 스킬(`tdd`)의 실제 산출물은 Python 테스트 코드(`unittest`)이지만, 조직 프로세스는 이와 별개로 **형식화된 단위시험 명세서·결과서(xlsx)**도 요구한다. 코드와 이 문서는 서로의 대체재가 아니라 같은 내용의 두 표현이다 — 둘이 어긋나면 안 된다.

## TPL-SWE4-001 — 시트 "Unit Test Specification" (고정 컬럼)
**Test ID | Trace | Unit/Feature | Technique | Input/Precondition | Expected Result | Automation | Test Function**

| 컬럼 | 채우는 방법 |
|------|--------------|
| Test ID | `TC-U-####` (단위시험 고유 ID) |
| Trace | 대상 단위 ID(`UNIT-####`, `detailed-design`) + 관련 `SWR-###` |
| Unit/Feature | 시험 대상 함수/단위명 |
| Technique | `test-case-design.md`의 기법(경계값분석/동등분할/오류추측/상태기반/회귀재현) + **긍정/부정**을 괄호로 병기(예: `경계값분석(긍정)`) |
| Input/Precondition | `@given`(테스트 함수 Doxygen 태그, `test-doxygen-convention.md`)과 동일 내용 |
| Expected Result | `@then` 태그와 동일 내용 |
| Automation | "자동"(unittest로 자동화됨 — 기본값) |
| Test Function | 실제 Python 테스트 함수 위치(`파일경로::TestClass::test_메서드명`) — 코드와 문서를 잇는 핵심 컬럼 |

## TPL-SWE4-001 — 시트 "Entry Exit Criteria"
컬럼: Criterion ID | 구분 | 기준 | 측정 방법 | 목표 | 책임 | 상태. `coding` 스킬의 5개 품질지표(라인수/복잡도/중복/주석비율/네이밍)를 진입·종료 기준으로 등록할 때 이 시트를 쓴다 — 예: "Criterion: 함수 순환복잡도" / "측정 방법: xenon" / "목표: ≤10".

## TPL-SWE4-002 — SW 단위시험 결과서
`coverage.py` 실행 결과(통과/실패, 커버리지 수치)를 이 결과서에 옮긴다.

## 작성 순서
1. `tdd` SKILL.md의 Red 단계에서 테스트 함수를 작성하면서 동시에 "Unit Test Specification" 시트의 해당 행을 채운다(같은 정보를 두 번 따로 설계하지 않고, 코드의 Doxygen 태그 내용을 그대로 표로 옮기는 것으로 처리한다).
2. Green/Refactor 완료 후 "Entry Exit Criteria" 시트에 품질지표 실측치를 기록한다.
3. 전체 스위트 실행 후 TPL-SWE4-002 결과서를 갱신한다.
