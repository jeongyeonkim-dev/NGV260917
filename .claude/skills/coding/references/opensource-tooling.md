# 오픈소스 측정 도구 (CLAUDE.md "측정 지표는 오픈소스 도구를 이용" 근거)

지표를 자체 판단하지 않고 아래 도구로 실제 측정한다. 프로젝트에 이미 다른 도구가 설치되어 있으면 그것을 우선 사용하고, 아래는 표준 후보다.

## 지표 ↔ 도구 매핑

| 지표 | 추천 도구 | 비고 |
|------|-----------|------|
| 함수 라인수 | `radon raw` | 함수별로는 직접 AST 분석 스크립트나 `radon cc`의 블록 단위 출력과 함께 확인 |
| 순환 복잡도 | `radon cc`, `ruff`(`C901`) | `radon cc`는 함수별 등급(A~F)과 수치를 함께 출력 |
| 중복 코드 | `pylint`(duplicate-code, `R0801`), `jscpd` | `min-similarity-lines=8`로 설정(중복 7라인까지 허용 기준과 정합) |
| 주석 비율 | `radon raw` | `comments`, `multi`(멀티라인 docstring), `sloc` 값으로 비율 계산 |
| 네이밍 | `pylint`(naming-style 옵션) | `references/python-style-and-naming.md`의 `.pylintrc` 예시 참조 |
| 테스트 커버리지(보조지표) | `coverage.py` | TDD 회귀 확인 시 함께 측정 권장 |

## 설치
```
pip install radon pylint ruff coverage
# jscpd는 Node 기반 도구이므로 Node.js 환경이 있을 때만 사용(선택)
npm install -g jscpd
```

## 실행 예시

### 순환 복잡도
```
radon cc -s -a src/            # 함수별 복잡도와 등급, 평균 표시
ruff check --select C901 src/  # ruff의 mccabe 기반 복잡도 검사(임계값은 pyproject.toml에서 설정)
```
`pyproject.toml` 설정 예:
```toml
[tool.ruff.mccabe]
max-complexity = 10
```

### 라인수·주석비율(raw metrics)
```
radon raw src/foo.py
```
출력 항목 중 `LOC`(총 라인), `LLOC`(논리 라인), `SLOC`(코드 라인), `Comments`(주석 라인), `Multi`(멀티라인 문자열/docstring), `Blank`(공백 라인)을 이용해:
- 주석 비율 = `(Comments + Multi) / (Comments + Multi + SLOC)`

### 중복 코드
```
pylint --disable=all --enable=duplicate-code --min-similarity-lines=8 src/
```
또는
```
jscpd --min-lines 8 --languages python src/
```

### 네이밍
```
pylint src/  # .pylintrc의 naming-style 설정을 함께 적용(아래 파일 참조)
```

### 테스트 실행·커버리지
```
python -m unittest discover -s tests
coverage run -m unittest discover -s tests
coverage report -m
```

## 결과 기록 원칙
각 실행 결과(수치, 위반 항목)를 구현 완료 보고에 그대로 포함한다 — "통과했다"는 서술만 남기지 않고, 실제 명령 출력(요약)을 근거로 남긴다.
