---
name: coding
description: Python 3.14로 구현할 때 CLAUDE.md의 품질지표(함수 라인수·순환복잡도·중복코드·Doxygen 주석비율·네이밍)와 A-SPICE SWE.3/ISO 26262-6 Clause 8 코딩원칙을 준수하는지 오픈소스 도구로 측정·강제할 때 사용. coding 서브에이전트가 tdd 스킬과 함께 사용하는 표준·품질 게이트 스킬.
---

# 구현 표준 스킬 (Python 3.14 · 품질지표 · A-SPICE/ISO 26262 코딩원칙)

CLAUDE.md 구현 지침에 명시된 기술 스택과 품질지표를 강제한다. 이 스킬은 "무엇을, 어떤 기준으로, 어떤 도구로 측정하는가"를 다루고, "어떤 순서로 작성하는가"는 `tdd` 스킬이 담당한다. 두 스킬은 함께 쓰인다 — 특히 `tdd`의 3단계(Refactor)에서 이 스킬의 품질 게이트를 실제로 측정한다.

## 기술 스택
- **언어/버전**: Python 3.14
- **테스트 프레임워크**: 표준 라이브러리 `unittest` (`tdd` 스킬 참조)
- **측정 도구**: 순환복잡도·라인수·중복·주석비율 모두 **오픈소스 도구로 실측**한다(자체 판단으로 "지켰다"고 넘어가지 않는다). 구체적 도구와 명령은 `references/opensource-tooling.md` 참조.

## 필수 품질지표 (CLAUDE.md 근거, 전부 "반드시" 준수)

| 지표 | 기준 | 측정 방법(요지) |
|------|------|-------------------|
| 함수 라인수 | 순수 코드라인(공백·주석 제외) 50라인 이하 | `radon raw` 또는 AST 기반 라인 카운트 — 함수별 SLOC 확인 |
| 순환 복잡도 | 함수당 10 이하 | `radon cc` 또는 `ruff --select C901`(mccabe) |
| 중복 코드 | 7라인까지 허용(8라인 이상 중복은 위반) | `pylint`(duplicate-code, `min-similarity-lines=8`) 또는 `jscpd --min-lines 8` |
| 주석 비율 | Doxygen 방식, 전체의 20% 이상 | `radon raw`의 comment 라인 수 / SLOC, Doxygen 태그 규칙은 `references/doxygen-comment-convention.md` |
| 네이밍 | 함수명·변수명 3글자 이상, 카멜케이스(camelCase) | `pylint` naming-style 설정(`references/python-style-and-naming.md`) — PEP8 기본값(snake_case)을 프로젝트 정책으로 의도적으로 재정의한 것임을 인지 |

각 지표의 상세 정의·근거·명령 예시는 `references/quality-metrics.md`에 있다.

## 절차

### 1단계 — 작성 시 규칙 적용 (tdd의 2~3단계와 병행)
- 네이밍 규칙(3글자 이상, 카멜케이스)은 코드를 처음 작성하는 순간부터 지킨다 — 나중에 일괄 변경하지 않는다.
- 모든 공개(외부에서 호출되는) 함수/메서드에는 작성 즉시 Doxygen 스타일 주석을 단다(`references/doxygen-comment-convention.md`). 나중에 몰아서 채우지 않는다 — 몰아서 채우면 `@param`/`@throws`가 실제 구현과 어긋나기 쉽다.
- A-SPICE SWE.3.BP3와 ISO 26262-6 Clause 8 코딩원칙(1진입/1탈출, 암묵적 타입변환 금지, 포인터/재귀 관련 항목은 Python 특성에 맞게 해석 — `references/aspice-iso26262-coding-link.md`)을 지킨다. 이 원칙들은 `detailed-design` 스킬의 `iso26262-6-unit-design-principles.md`와 동일한 근거를 공유한다.

### 2단계 — 품질 게이트 실측 (tdd의 Refactor 단계에서 수행)
`references/opensource-tooling.md`의 명령으로 5개 지표를 모두 실제로 측정한다. 측정 없이 통과로 간주하지 않는다. 기준을 벗어나면:
- 라인수 초과 → 함수를 책임 단위로 분리(`architecture-designer`/`detailed-design`의 SRP·응집도 원칙과 동일한 논리를 함수 수준에 적용).
- 복잡도 초과 → 분기·중첩을 줄이거나(가드절, 조기 반환), 하위 함수로 추출.
- 중복 초과(8라인 이상) → 공통 로직을 함수/모듈로 추출.
- 주석 비율 미달 → Doxygen 주석 보완(단, 코드를 읽으면 뻔한 내용을 억지로 채우는 "주석을 위한 주석"은 지양하고, 계약(사전/사후조건)·예외·설계 근거 위주로 작성).
- 네이밍 위반 → 린트 도구가 잡아낸 식별자를 규칙에 맞게 수정.

### 3단계 — 코드-상세설계 일관성 확인
구현된 코드가 `detailed-design` 산출물의 인터페이스 명세(파라미터, 유효범위, 오류처리)와 실제로 일치하는지 확인한다. 상세설계와 다르게 구현해야 하는 불가피한 사유가 생기면, 코드를 조용히 다르게 만들지 말고 상세설계 문서를 먼저 갱신한다(추적성 일관성 유지, SWE.3 Note 10과 동일한 원칙).

### 4단계 — 결과 기록
구현 완료 시 아래를 보고한다: 대상 단위 ID, 실행한 측정 도구와 결과값(지표별 실측치), 기준 초과로 재작업한 항목, 코드 위치(파일경로::함수명) — 이는 `detailed-design`/`tdd` 스킬의 추적성 매트릭스 갱신에 그대로 쓰인다.

## 참고 자료
- `references/quality-metrics.md` — 5개 지표 상세 정의·근거
- `references/opensource-tooling.md` — 도구별 설치·실행 명령, 지표-도구 매핑표
- `references/python-style-and-naming.md` — 네이밍 규칙과 pylint 설정 예시
- `references/doxygen-comment-convention.md` — Python용 Doxygen 주석 작성법과 예시
- `references/aspice-iso26262-coding-link.md` — A-SPICE SWE.3.BP3 / ISO 26262-6 Clause 8과의 연결

## 주의사항
- 이 프로젝트의 네이밍 규칙(카멜케이스)은 PEP8 표준(스네이크케이스)과 다르다 — 이것은 실수가 아니라 CLAUDE.md의 의도적 정책이다. 다른 파이썬 프로젝트의 관행을 따라 스네이크케이스로 되돌리지 않는다.
- 품질지표는 "대략 지킨 것 같다"가 아니라 도구 실행 결과 수치로 확인·기록한다.
