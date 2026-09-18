# 네이밍 규칙과 린트 설정 (카멜케이스, 3글자 이상)

CLAUDE.md 지침: "함수명, 변수명은 3글자 이상 사용하고, 낙타 표기법을 활용한다." 이는 PEP8의 기본 권장(스네이크케이스)을 프로젝트 정책으로 **의도적으로 재정의**한 것이다 — 다른 파이썬 프로젝트의 관행을 이유로 되돌리지 않는다.

## 규칙 요약
- 함수명: `camelCase`, 최소 3글자 이상 (예: `calcSpeed`, `getUserName`) — `get`, `set`처럼 2글자 이하 단독 이름은 금지.
- 변수명: `camelCase`, 최소 3글자 이상 (예: `speedValue`, `idx`, `row`) — `i`, `x`, `dt` 같은 1~2글자 이름 금지, 관례적 예외는 사유를 주석으로 남기고 최소화.
- 클래스명: 관례상 `PascalCase`를 유지한다(카멜케이스 규칙은 함수·변수 대상이며, 클래스는 파이썬/일반적 OOP 관례를 따른다). 프로젝트에서 다르게 정하고 싶으면 사용자에게 확인한다.
- 상수: 관례상 `UPPER_CASE`를 유지할지, 이 프로젝트 규칙(카멜케이스)을 상수에도 적용할지는 프로젝트에서 명시적으로 정하지 않았다 — 사용자에게 확인 후 이 파일에 결정사항을 추가한다.

## `.pylintrc` 설정 예시
```ini
[BASIC]
function-naming-style=camelCase
variable-naming-style=camelCase
argument-naming-style=camelCase
attr-naming-style=camelCase
method-naming-style=camelCase

; 최소 3글자 이상을 강제하는 정규식 (camelCase, 3자 이상)
function-rgx=^[a-z][a-zA-Z0-9]{2,}$
variable-rgx=^[a-z][a-zA-Z0-9]{2,}$
argument-rgx=^[a-z][a-zA-Z0-9]{2,}$
attr-rgx=^[a-z][a-zA-Z0-9]{2,}$
method-rgx=^[a-z][a-zA-Z0-9]{2,}$
```
- 정규식 `^[a-z][a-zA-Z0-9]{2,}$`는 "소문자로 시작하고, 그 뒤로 영숫자가 2글자 이상"이므로 전체 길이 최소 3글자를 보장한다.
- `pylint`는 `function-naming-style`에 `camelCase` 값을 직접 지원하지만, 최소 글자수까지는 `-rgx` 옵션으로 별도 지정해야 한다(스타일 옵션만으로는 길이를 강제하지 못한다).

## 검증
```
pylint src/
```
`invalid-name` 경고가 나오면 규칙 위반 식별자를 수정한다. CI에 편입할 경우 `pylint --fail-under=<임계점>`으로 게이트를 걸 수 있다(다만 이 프로젝트의 필수 게이트는 `quality-metrics.md`의 5개 지표이며, pylint 종합 점수 자체를 별도 기준으로 삼을지는 사용자와 협의한다).
