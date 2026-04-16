# harness_JM

이 저장소는 다른 프로젝트를 시작할 때 바로 복제해서 쓸 수 있는 하네스 템플릿이다.
원본 `saju_harness_practice`에서 하네스 자체에 해당하는 구조만 남기고, 앱 예시는 일반화했다.

## Included
- `harness/`: 저장소 공통 하네스 문서, 역할 정의, 워크플로, 스크립트
- `AGENTS.md`: Codex 에이전트 진입 가이드
- `apps/sample-app/`: 새 앱을 시작할 때 참고할 app-level harness 골격

## How To Use
1. 이 저장소를 새 프로젝트 시작점으로 복제한다.
2. `apps/sample-app/`를 복사해서 `apps/<your-app-name>/`로 이름을 바꾼다.
3. 각 문서에서 `sample-app`과 placeholder를 실제 프로젝트 내용으로 바꾼다.
4. 작업 시작 전 `AGENTS.md`와 `harness/core/docs/index.md`부터 읽는다.

## Recommended Start Order
1. `AGENTS.md`
2. `harness/core/docs/index.md`
3. `harness/core/workflows/pipeline.md`
4. `apps/<app-name>/harness/docs/index.md`
5. `apps/<app-name>/harness/plans/tracker.md`

## Notes
- `harness/scripts/`의 cleanup/checkpoint 스크립트는 그대로 포함했다.
- `harness/runtime/`는 실행 중 생기는 artifact 자리이며 source of truth가 아니다.
- 실제 앱 구현 코드는 각 `apps/<app-name>/src/`와 `tests/` 아래에 둔다.
- 이 템플릿은 Codex 기준으로 진입점을 정리했다.
