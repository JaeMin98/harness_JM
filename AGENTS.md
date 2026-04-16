# Repository Agent Guide

## Purpose
이 문서는 Codex가 이 저장소에서 작업을 시작할 때 읽는 루트 진입점이다.

## Read Order
1. `harness/core/docs/index.md`
2. `harness/core/workflows/pipeline.md`
3. 작업할 프로젝트의 `apps/<project-name>/harness/docs/index.md`
4. 작업할 프로젝트의 `apps/<project-name>/harness/plans/tracker.md`

## Working Rule
- repository-level 규칙은 `harness/core/`를 따른다.
- project-level 규칙은 해당 프로젝트의 `harness/` 문서를 따른다.
- 짧은 사용자 요청은 먼저 planner 관점으로 spec과 roadmap으로 확장한다.
- 구현 전에는 generator와 evaluator가 sprint contract를 합의한다.
- 상태 변경이 있으면 `tracker.md`와 ongoing artifact를 갱신한다.
- handoff 전에는 cleanup agent를 실행한다.
- risky cleanup이나 큰 변경 전에는 git checkpoint를 먼저 만든다.

## Entry Hint
- 새 프로젝트를 시작할 때는 `apps/sample-project/`를 복사해 `apps/<project-name>/`으로 사용한다.
- 사람 기준 안내는 각 프로젝트의 `README.md`에 둔다.
- project-level 진입점은 `apps/<project-name>/harness/docs/index.md`다.
