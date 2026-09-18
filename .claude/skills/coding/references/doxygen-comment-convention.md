# Python용 Doxygen 주석 작성법

Doxygen은 C/C++/Java 계열 언어에 흔히 쓰이지만 Python 파일도 네이티브로 인식한다. 이 프로젝트는 CLAUDE.md 지침에 따라 모든 공개 함수/클래스에 Doxygen 스타일 주석을 단다.

## 기본 형식
docstring을 `"""!`로 시작하면 Doxygen이 이를 문서화 주석으로 인식한다.

```python
def calcSpeed(distanceMeter: float, timeSecond: float) -> float:
    """!
    @brief 이동 거리와 시간으로 평균 속도를 계산한다.

    @param distanceMeter 이동 거리. 유효범위 0..10000 [m].
    @param timeSecond 소요 시간. 유효범위 0(제외)..3600 [s].
    @return 평균 속도 [m/s]. 유효범위 0..200.
    @throws ValueError timeSecond가 0 이하이거나, 계산 결과가 유효범위(0..200)를 벗어나는 경우.

    @pre timeSecond > 0 이어야 한다.
    @post 반환값은 항상 0 이상 200 이하이다.
    """
    if timeSecond <= 0:
        raise ValueError("timeSecond must be greater than 0")
    speedValue = distanceMeter / timeSecond
    if not (0 <= speedValue <= 200):
        raise ValueError("calculated speed out of valid range")
    return speedValue
```

## 필수 태그
| 태그 | 용도 | 생략 가능 조건 |
|------|------|------------------|
| `@brief` | 함수/클래스의 한 줄 요약 | 없음(항상 작성) |
| `@param` | 각 파라미터의 의미, 유효범위, 물리단위 | 파라미터가 있으면 항상 작성 — `detailed-design`의 단위 인터페이스 명세를 그대로 옮긴다 |
| `@return` | 반환값의 의미, 유효범위, 물리단위 | 반환값이 있으면 항상 작성 |
| `@throws`(또는 `@exception`) | 발생 가능한 예외와 조건 | 예외를 던지는 경우 항상 작성 |
| `@pre` / `@post` | 사전조건/사후조건 | `detailed-design`에 사전/사후조건이 정의된 경우 작성 |

## 클래스 주석 예시
```python
class SpeedCalculator:
    """!
    @brief 센서 원시값으로부터 평균 속도를 계산하는 컴포넌트.

    상세설계 문서: UNIT-CompA-001 참조.
    """
```

## 작성 원칙
- **코드 재진술 금지**: "i를 1 증가시킨다" 같은, 코드를 읽으면 바로 아는 내용은 쓰지 않는다.
- **계약과 근거 우선**: 유효범위·물리단위·예외조건·설계 결정 근거(왜 이렇게 구현했는지)처럼 코드만 봐서는 알 수 없는 정보를 우선 채운다.
- **상세설계와 동기화**: 파라미터의 유효범위·단위는 `detailed-design` 산출물과 반드시 일치해야 한다 — 둘이 어긋나면 상세설계를 먼저 갱신한 뒤 주석도 함께 고친다.
- **Doxyfile 설정**(문서 생성까지 필요할 경우): `FILE_PATTERNS`에 `*.py`를 포함하고 `JAVADOC_AUTOBRIEF = YES`를 설정하면 `@brief` 없이 첫 문장을 요약으로 인식하지만, 이 프로젝트는 명시적으로 `@brief`를 쓰는 것을 기본으로 한다(가독성·일관성 우선).

## 주석 비율 측정과의 연결
`opensource-tooling.md`의 `radon raw` 결과에서 `Multi`(멀티라인 docstring, 즉 이 Doxygen 주석 블록)와 `Comments`(`#` 라인 주석) 합계를 분자로 사용해 20% 기준을 확인한다.
