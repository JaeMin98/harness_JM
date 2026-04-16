# Sample App Agent Workflow

## Purpose
이 문서는 `sample-app` 작업에 repository-level pipeline을 어떻게 적용할지 정의한다.

## Workflow Rule
- 기본 pipeline은 `PM -> Coder -> Security Reviewer -> Tester -> PM` 순서를 따른다.
- UI나 사용자 흐름이 크게 바뀌면 `Designer`를 `PM` 다음에 추가한다.
- handoff 기록은 app-level plans 문서에 남긴다.

## Cleanup Rule
- 기본 cleanup 실행은 `python3 harness/scripts/cleanup_agent.py apps/sample-app`를 사용한다.
- 프로세스 조회가 막힌 환경에서는 `python3 harness/scripts/cleanup_agent.py --skip-processes apps/sample-app`를 사용하고 그 사실을 기록한다.
