# Sample Project Agent Workflow

## Purpose
이 문서는 `sample-project` 작업에 repository-level pipeline을 어떻게 적용할지 정의한다.

## Workflow Rule
- 기본 pipeline은 `Planner -> Design Critic(requested only) -> Generator -> Security Reviewer -> Evaluator -> Planner` 순서를 따른다.
- UI나 사용자 흐름 작업은 사용자가 명시적으로 요청한 경우에만 Design Critic을 참여시킨다.
- 구현 시작 전에는 Generator와 Evaluator가 sprint contract를 먼저 합의한다.
- handoff 기록은 project-level plans 문서에 남긴다.

## Contract Rule
각 sprint contract에는 아래 항목을 남긴다.
- sprint goal
- single feature for this turn
- in scope
- out of scope
- done criteria
- feature test criteria
- integration test criteria
- security concerns
- design criteria if explicitly requested

## Cleanup Rule
- 기본 cleanup 실행은 `python3 harness/scripts/cleanup_agent.py apps/sample-project`를 사용한다.
- 프로세스 조회가 막힌 환경에서는 `python3 harness/scripts/cleanup_agent.py --skip-processes apps/sample-project`를 사용하고 그 사실을 기록한다.
