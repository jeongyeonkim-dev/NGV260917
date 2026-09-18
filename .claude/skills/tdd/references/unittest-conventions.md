# Python `unittest` 작성 규칙

CLAUDE.md 지침에 따라 코드 테스트는 표준 라이브러리 `unittest`를 사용한다(추가 설치 불필요, Python 3.14 표준 배포에 포함).

## 파일·클래스 구조
- 테스트 파일은 `test_<대상모듈>.py` 형식으로, 대상 모듈과 대응되는 위치(예: `tests/` 디렉터리, 프로젝트 구조에 맞춰 결정)에 둔다.
- 테스트 클래스는 `unittest.TestCase`를 상속하고, 클래스명은 `Test<대상단위명>`(카멜케이스, `coding` 스킬의 네이밍 규칙 준수)으로 짓는다.
- 테스트 메서드명은 `test_` 접두사 규칙(unittest 프레임워크 요구사항)을 따르되, 이어지는 부분은 "무엇을 검증하는지"가 드러나게 짓는다: `test_유효범위내값_정상반환`처럼 한국어 설명을 붙여도 되고, `test_normalRange_returnsOk`처럼 영문 카멜케이스로 지어도 된다 — 프로젝트 내에서 하나의 방식으로 통일한다.

## 기본 골격
**모든 테스트 메서드에는 Doxygen 형식의 목적 설명(`@brief`/`@testtype`/`@technique`)을 반드시 단다 — `test-doxygen-convention.md` 참조.**

```python
import unittest

class TestCalcSpeed(unittest.TestCase):
    def setUp(self):
        # 각 테스트 전에 공통으로 필요한 준비 작업
        pass

    def test_validRange_returnsExpected(self):
        """!
        @brief 유효범위 내 대표값을 입력하면 그대로 반환하는지 검증한다.
        @testtype 긍정(Positive)
        @technique 동등분할(Equivalence Partitioning) — 정상 카테고리 대표값
        """
        # Arrange - Act - Assert 구조를 지킨다
        result = calcSpeed(100)
        self.assertEqual(result, 100)

    def test_belowMinBoundary_raisesValueError(self):
        """!
        @brief 유효범위 최소값 미만 입력 시 ValueError가 발생하는지 검증한다.
        @testtype 부정(Negative)
        @technique 경계값분석(Boundary Value Analysis) — 최소값 바로 아래 값
        """
        with self.assertRaises(ValueError):
            calcSpeed(-1)

    def tearDown(self):
        # 필요한 경우 정리 작업
        pass

if __name__ == "__main__":
    unittest.main()
```

## 경계값·예외 테스트 작성 규칙
- 상세설계의 유효범위가 `0..200`이면 최소한 다음 케이스를 각각 별도 테스트 메서드로 작성한다: `0`(최소 경계), `200`(최대 경계), `-1`(최소 미만), `201`(최대 초과), 대표 정상값(예: `100`).
- 사전조건이 있으면(예: "초기화가 완료된 상태여야 함") 사전조건을 위반한 호출에 대한 테스트도 포함한다.
- `assertEqual`, `assertTrue`/`assertFalse`, `assertRaises`, `assertAlmostEqual`(부동소수점 비교) 등 상황에 맞는 assert 메서드를 사용한다 — `assertTrue(a == b)`처럼 범용 assert로 뭉뚱그리지 않는다(실패 메시지의 정보량이 줄어든다).

## 실행과 커버리지
- 개별 실행: `python -m unittest tests.test_calc_speed`
- 전체 스위트 실행: `python -m unittest discover -s tests`
- 커버리지 측정은 `coding` 스킬의 `references/opensource-tooling.md`에 정리된 도구(예: `coverage.py`)를 사용한다: `coverage run -m unittest discover && coverage report`
