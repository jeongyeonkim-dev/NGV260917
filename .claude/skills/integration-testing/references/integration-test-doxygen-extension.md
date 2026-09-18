# 통합시험 전용 Doxygen 태그 확장

`tdd` 스킬의 `references/test-doxygen-convention.md`(모든 테스트에 `@brief`/`@testtype`/`@technique` 필수)를 통합시험에도 그대로 적용하고, 아래 태그를 추가한다.

| 태그 | 값 | 필수 여부 |
|------|-----|-----------|
| `@integrationstep` | `integration-sequence.md`의 단계 번호와 대상 컴포넌트 | 통합시험은 항상 필수 |
| `@interface` | 시험 대상 인터페이스 이름(아키텍처 인터페이스 명세의 이름과 동일하게) | 인터페이스시험이면 필수 |
| `@coveragetarget` | 이 시험이 커버하려는 함수명 또는 호출 지점(파일:라인) | 커버리지 보강 목적으로 추가한 시험은 필수 |

## 작성 예시

```python
def test_step2_compAtoCompB_interfaceReturnsWithinRange(self):
    """!
    @brief 2단계 통합에서 Comp-A가 Comp-B의 getSpeed 인터페이스를 호출했을 때
           반환값이 유효범위(0..200) 내에 있는지 검증한다.
    @testtype 긍정(Positive)
    @technique 경계값분석(Boundary Value Analysis) — 유효범위 최대값 근접
    @integrationstep 2단계 — Comp-A, Comp-B 통합
    @interface CompB.getSpeed
    """
    result = compA.requestSpeedFromCompB()
    self.assertTrue(0 <= result <= 200)

def test_step2_compAtoCompB_faultInjection_timeoutTriggersFailSafe(self):
    """!
    @brief Comp-B 응답 타임아웃(결함주입) 시 Comp-A가 안전상태로 전이하는지 검증한다.
    @testtype 부정(Negative)
    @technique 오류추측(Error Guessing) + 결함주입시험(Fault Injection Test, ISO 26262-6)
    @integrationstep 2단계 — Comp-A, Comp-B 통합
    @interface CompB.getSpeed
    @coveragetarget compA.py:onTimeoutFailSafe (통합시험으로만 도달 가능한 오류처리 경로)
    """
    with simulateTimeout(compB):
        self.assertEqual(compA.state, "FAIL_SAFE")
```

## 점검 규칙
- `@integrationstep`이 없는 통합시험은 테스트 베이시스(통합순서)와 연결되지 않은 것으로 간주하고 보완한다.
- `@coveragetarget`을 단 시험은 `coverage-100-percent-strategy.md`의 보고 표에서 해당 항목의 "보강 완료" 근거로 직접 인용한다.
