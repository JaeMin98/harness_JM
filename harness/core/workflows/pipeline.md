# Pipeline Workflow

## 1. Purpose
이 문서는 long-running Codex harness의 기본 작업 흐름을 정의한다.
Anthropic의 planner-generator-evaluator 패턴을 참고해, spec 작성, contract 합의, 구현, 회고를 분리한다.

## 2. Default Order
기본 흐름은 아래 순서를 따른다.

1. Planner
2. Design Critic (requested only)
3. Generator
4. Security Reviewer
5. Evaluator
6. Planner

## 3. Core Principles
- 짧은 사용자 요청은 먼저 planner가 spec과 roadmap으로 확장한다.
- 구현은 sprint 안에서도 한 번에 한 기능씩만 진행한다.
- generator와 evaluator는 구현 전에 sprint contract를 먼저 합의한다.
- contract에는 scope, non-goals, done criteria, test criteria를 남긴다.
- communication은 파일 기반 artifact로 남겨 다음 session이 바로 이어받게 한다.
- context가 흐려지면 compact만 고집하지 말고 handoff artifact와 함께 reset도 고려한다.
- harness 복잡도는 고정값이 아니며, 모델이 잘하는 영역에서는 더 단순하게 유지한다.
- UI와 디자인 단계는 사용자가 명시적으로 요청했을 때만 활성화한다.

## 4. Sprint Contract Rule
각 sprint 시작 전에 최소 아래 항목을 문서화한다.

- sprint goal
- single feature for this implementation turn
- in scope
- out of scope
- implementation notes
- feature test criteria
- integration test criteria
- security concerns
- design criteria if explicitly requested

Generator는 계약안을 제안하고, Evaluator는 검증 가능성과 누락 여부를 확인한다.
합의가 끝나기 전에는 구현을 확정하지 않는다.

## 5. Evaluation Rule
- Evaluator는 product depth, functionality, visual design, code quality를 기준으로 본다.
- 기준은 가능하면 PASS or FAIL로 남긴다.
- 기능별 테스트와 통합 테스트를 구분해 남긴다.
- 중요한 기준 하나라도 임계값 아래면 sprint는 실패다.
- feedback은 재현 가능하고 다음 수정으로 연결 가능해야 한다.

## 6. Handoff Rule
- 각 단계는 다음 단계가 바로 읽을 수 있는 artifact를 남긴다.
- tracker와 ongoing artifact는 현재 상태, 남은 리스크, 다음 액션을 포함한다.
- risky cleanup이나 큰 변경 전에는 git checkpoint를 먼저 만든다.
- handoff 전에는 cleanup 상태를 확인한다.
- 프로세스 조회가 막힌 환경에서는 `--skip-processes` 사용 여부를 함께 기록한다.

## 7. Completion Rule
- 마지막 planner 검토까지 끝나야 한 사이클이 완료된다.
- 마지막 planner 판정이 `APPROVED`일 때만 완료로 표시한다.
- model capability가 충분해 평가 단계가 과한 비용만 만든다면 evaluator를 축소할 수 있다.
- 단, generator solo 성능 경계 바깥 작업에서는 evaluator를 유지한다.
