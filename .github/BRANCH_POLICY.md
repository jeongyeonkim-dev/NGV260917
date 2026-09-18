# 브랜치 정책 (GitHub Flow 기반)

AI(Claude Code)가 커밋의 상당 부분을 작성하는 저장소이므로, 실수가 `main`에 바로 반영되지 않도록 **PR 필수 + CI 통과 필수**를 핵심으로 하는 단순한 GitHub Flow를 적용한다. 릴리스 브랜치·develop 브랜치처럼 무거운 구조(GitFlow)는 쓰지 않는다 — 이 프로젝트 규모와 반복 속도에는 과하다.

## 브랜치 구조
- **`main`**: 항상 배포 가능한 상태를 유지하는 유일한 장기 브랜치. 보호됨(아래 참조).
- **작업 브랜치**: `main`에서 분기해 짧게 유지하고, 작업이 끝나면 PR로 병합 후 삭제한다. 네이밍:
  - `feature/<설명>` — 새 기능
  - `fix/<설명>` — 버그 수정
  - `docs/<설명>` — 문서(CLAUDE.md, 스킬 문서 등)만 변경
  - `test/<설명>` — 테스트만 추가/보강
  - `chore/<설명>` — 빌드/CI/설정 등 그 외 잡무
  - 예: `feature/swe1-requirements-parser`, `fix/speed-calc-boundary`

## `main` 브랜치 보호 규칙
GitHub 저장소 설정(Settings → Branches → `main`)에 아래를 적용한다(적용 명령은 `.github/BRANCH_PROTECTION_SETUP.md` 참조):
- **PR을 통해서만 병합 가능** — `main`에 직접 push 금지(관리자 포함).
- **상태 검사(CI) 통과 필수** — `.github/workflows/ci.yml`의 `lint`, `test`, `status-check` 잡이 모두 성공해야 병합 가능. 브랜치가 최신 `main` 기준으로 업데이트되어 있어야 한다(`Require branches to be up to date before merging`).
- **리뷰 승인**: 현재 단독 개발이므로 승인 인원수는 0으로 둔다(대신 CI 통과가 게이트 역할을 한다). 협업자가 합류하면 1명 이상 승인을 요구하도록 즉시 상향한다.
- **강제 push·삭제 금지**(`main` 대상).
- **대화 스레드 해결 후 병합**(Require conversation resolution before merging) — 리뷰 코멘트가 있으면 해결 후 병합.
- **선형 히스토리 권장**(Require linear history) — 병합 시 squash merge를 기본으로 사용해 `main` 히스토리를 단순하게 유지한다.

## 커밋/PR 컨벤션
- 커밋 메시지는 [Conventional Commits](https://www.conventionalcommits.org/) 형식을 권장한다: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:` 접두사.
- PR은 `.github/pull_request_template.md` 템플릿을 채운다(어느 단계 산출물인지, 품질지표 실측 여부, 추적성 갱신 여부 체크리스트 포함).
- 병합 방식은 **Squash and merge**를 기본으로 한다 — 작업 브랜치의 자잘한 커밋을 하나로 정리해 `main` 히스토리를 읽기 쉽게 유지한다.
- 병합 후 작업 브랜치는 삭제한다("Automatically delete head branches" 저장소 설정 사용 권장).

## CI와의 연결
- `pull_request`(main 대상) 및 `push`(main) 이벤트에서 `.github/workflows/ci.yml`이 자동 실행된다.
- `lint` 잡: `ruff`, `xenon`(순환복잡도 ≤10, `coding` 스킬 품질지표 근거), `pylint`.
- `test` 잡: `unittest` 실행과 `coverage` 측정.
- 두 잡을 모두 성공해야 하는 `status-check` 잡을 브랜치 보호의 필수 상태 검사로 지정한다.
- 프로젝트에 소스 코드가 아직 없는 동안에는 각 단계가 "건너뜀" 알림만 내고 성공 처리된다 — 코드가 추가되면 자동으로 실제 검사가 활성화된다.

## 협업자가 늘어나면 바꿀 것
- 리뷰 승인 최소 인원수를 1명 이상으로 상향.
- 필요 시 `CODEOWNERS` 파일을 추가해 영역별(예: `.claude/skills/` 변경은 특정 인원 필수 리뷰 등) 리뷰를 강제.
