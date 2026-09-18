# 테스트 함수의 Doxygen 형식 목적 설명 (필수)

이 프로젝트의 모든 테스트 함수는 `coding` 스킬의 `doxygen-comment-convention.md`(프로덕션 코드용)와 짝을 이루는 **테스트 전용 Doxygen 규칙**을 따른다. 목적은 세 가지다:
1. 이 테스트가 **왜** 존재하는지(무엇을 검증하는지) 코드만 봐도 알 수 있게 한다.
2. 이 테스트가 **어떤 기법**(`test-case-design.md`)으로 도출되었는지 기록해, 커버리지 논리를 추적할 수 있게 한다.
3. 이 테스트가 **긍정(정상)**인지 **부정(오류·경계·예외)**인지 한눈에 구분해, 케이스 분포(정상만 있고 오류 케이스가 없는지 등)를 점검할 수 있게 한다.

## 필수 태그

| 태그 | 값 | 필수 여부 |
|------|-----|-----------|
| `@brief` | 이 테스트가 검증하는 행위를 한 문장으로 | 항상 필수 |
| `@testtype` | `긍정(Positive)` 또는 `부정(Negative)` | 항상 필수 |
| `@technique` | 경계값분석(Boundary Value Analysis) / 동등분할(Equivalence Partitioning) / 오류추측(Error Guessing) / 상태기반테스트(State-based Testing) / 회귀재현(Regression) 중 하나(또는 조합) | 항상 필수 |
| `@given` | 테스트의 입력·사전조건 요약 | 권장 |
| `@then` | 기대하는 결과 요약 | 권장 |

`@testtype`, `@technique`, `@given`, `@then`은 Doxygen 표준 태그가 아니라 이 프로젝트의 관례다. 실제로 Doxygen 문서를 생성한다면 `Doxyfile`에 아래처럼 별칭(ALIASES)을 등록해 정상적으로 렌더링한다(문서를 생성하지 않아도 사람이 읽는 구조화된 주석으로서 유효하다):

```
ALIASES += testtype="@par 케이스 성격:"
ALIASES += technique="@par 사용 기법:"
ALIASES += given="@par 전제조건(Given):"
ALIASES += then="@par 기대결과(Then):"
```

## 작성 예시

### 긍정 케이스 (경계값 분석)
```python
def test_atMinBoundary_returnsMinResult(self):
    """!
    @brief 입력이 유효범위의 최소 경계값일 때 정상적으로 처리되는지 검증한다.
    @testtype 긍정(Positive)
    @technique 경계값분석(Boundary Value Analysis) — 유효범위 0..200의 최소값(0) 사용
    @given distanceMeter=0, timeSecond=10
    @then 반환값은 0이어야 하며 예외가 발생하지 않아야 한다.
    """
    result = calcSpeed(0, 10)
    self.assertEqual(result, 0)
```

### 부정 케이스 (경계값 분석 + 오류추측)
```python
def test_belowMinBoundary_raisesValueError(self):
    """!
    @brief 유효범위 최소값 미만 입력을 받으면 ValueError를 발생시키는지 검증한다.
    @testtype 부정(Negative)
    @technique 경계값분석(Boundary Value Analysis) — 최소값(min) 바로 아래 값 사용
    @given timeSecond=-1 (유효범위는 0 초과)
    @then ValueError가 발생해야 한다.
    """
    with self.assertRaises(ValueError):
        calcSpeed(10, -1)
```

### 부정 케이스 (회귀재현)
```python
def test_emptyEmail_rejectedAfterBugfix(self):
    """!
    @brief 빈 이메일 문자열이 통과되던 회귀 버그(#123)가 재발하지 않는지 검증한다.
    @testtype 부정(Negative)
    @technique 회귀재현(Regression) — 보고된 결함 재현 케이스
    @given email=""
    @then result.error == "Email required"
    """
    result = submitForm({"email": ""})
    self.assertEqual(result.error, "Email required")
```

## 점검 규칙
- `@technique`이 없는 테스트는 "왜 이 테스트가 존재하는지"를 증명하지 못한 것으로 간주하고 보완한다.
- 하나의 단위에 긍정 케이스만 있고 부정 케이스가 하나도 없다면(또는 그 반대) 미완성으로 표시한다 — `tdd` 스킬 SKILL.md의 Red 단계는 정상·경계·오류 케이스를 함께 요구한다.
- `@brief`는 테스트 함수명과 중복되는 뻔한 설명("성공 케이스를 테스트한다")이 아니라, 함수명만으로 드러나지 않는 맥락(왜 이 값을 선택했는지, 어떤 요구사항/상세설계 항목과 연결되는지)을 담는다.
