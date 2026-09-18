# 출처 표시 (Attribution)

이 스킬(`tdd`)의 `SKILL.md`와 `references/writing-good-tests.md`는 아래 오픈소스 프로젝트의 내용을 골격으로 삼아, Python/unittest 환경과 이 프로젝트(요구사항→아키텍처→상세설계→구현) 워크플로우에 맞게 번역·각색·확장한 것이다.

- 원본: [obra/superpowers](https://github.com/obra/superpowers), `skills/test-driven-development/SKILL.md`, `skills/test-driven-development/writing-good-tests.md`
- 라이선스: MIT License
- 원저작권: Copyright (c) 2025 Jesse Vincent

## MIT License 전문

```
MIT License

Copyright (c) 2025 Jesse Vincent

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 이 프로젝트에서 추가/변경한 부분 (원본에 없던 것)
- 0단계(상세설계 산출물 확인)와 마지막 추적성 갱신 단계 — `detailed-design`/`architecture-designer`/`requirements-analyst` 스킬과의 연동.
- Refactor 단계에 `coding` 스킬의 5개 품질지표(라인수/복잡도/중복/주석비율/네이밍) 실측 의무화.
- 모든 테스트에 Doxygen 형식 목적 설명(`@brief`/`@testtype`/`@technique`) 필수화 — `test-doxygen-convention.md`.
- 테스트 케이스 설계 기법(경계값분석/동등분할/오류추측/상태기반/회귀재현)과 긍정/부정 케이스 분류 — `test-case-design.md`.
- 예시 코드를 TypeScript/Jest에서 Python/unittest로 변환.
