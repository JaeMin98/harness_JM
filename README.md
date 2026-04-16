# harness_JM

이 저장소는 다른 프로젝트를 시작할 때 바로 복제해서 쓸 수 있는 Codex용 하네스 템플릿이다.
원본 `saju_harness_practice`에서 하네스 자체에 해당하는 구조만 남기고, 새 프로젝트에 맞게 일반화했다.

Anthropic의 [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) 글을 참고해 역할을 `Planner -> Generator -> Evaluator` 중심으로 미세조정했다. 이 저장소에서는 기존 PM, Coder, Tester 역할을 각각 그 관점에 맞게 다듬고, Design Critic과 Security Reviewer를 보조 심사축으로 유지한다.

## Included
- `harness/`: 저장소 공통 하네스 문서, 역할 정의, 워크플로, 스크립트
- `AGENTS.md`: Codex 에이전트 진입 가이드
- `apps/sample-project/`: 새 프로젝트를 시작할 때 참고할 project-level harness 골격

## Role Model
- `Planner`: 짧은 요구를 spec, roadmap, chunk로 확장한다.
- `Generator`: 합의된 sprint contract를 기준으로 한 번에 한 기능씩 구현한다.
- `Evaluator`: 기능별 테스트와 통합 테스트를 나눠 검증하고 fail/pass를 판단한다.
- `Design Critic`: UI를 사용자가 명시적으로 요청했을 때만 coherence, originality, craft, functionality 기준을 만든다.
- `Security Reviewer`: 민감 데이터, 권한, 노출 위험을 별도 축으로 검토한다.

## How To Use This Template
1. 이 저장소를 템플릿처럼 복제해 새 레포를 만든다.
2. `apps/sample-project/`를 복사해서 `apps/<your-project-name>/`로 이름을 바꾼다.
3. 각 문서의 placeholder를 실제 프로젝트 정보로 채운다.
4. Codex가 작업을 시작하기 전에 `AGENTS.md`, `harness/core/docs/index.md`, `harness/core/workflows/pipeline.md`를 먼저 읽게 한다.

## Recommended Start Order
1. `AGENTS.md`
2. `harness/core/docs/index.md`
3. `harness/core/workflows/pipeline.md`
4. `apps/<project-name>/harness/docs/index.md`
5. `apps/<project-name>/harness/plans/tracker.md`

## Starting a New Repository From This Template
새 프로젝트를 시작할 때는 보통 아래 순서가 가장 깔끔하다.

```powershell
# 1. 템플릿 복제
 git clone https://github.com/JaeMin98/harness_JM.git my-new-project
 cd my-new-project

# 2. 새 원격 연결 준비
 git remote remove origin
 git remote add origin https://github.com/<your-id>/<your-new-repo>.git

# 3. sample-project를 실제 프로젝트 이름으로 복사
 Copy-Item -Recurse apps/sample-project apps/<your-project-name>
```

원하면 `apps/sample-project`는 남겨둔 채 참고용으로 두고, 새 프로젝트 폴더만 추가해도 된다.

## How To Instruct Codex In A Fresh Project
새 레포에서 Codex에게는 처음부터 긴 구현 지시를 한 번에 주기보다, 아래처럼 단계적으로 시키는 편이 안정적이다.

### 1. Planner 호출
프로젝트를 처음 열었을 때:

```text
이 레포는 harness_JM 템플릿으로 시작했다. 먼저 AGENTS.md, harness/core/docs/index.md, harness/core/workflows/pipeline.md, apps/<project-name>/harness/docs/index.md 를 읽고, 내 요청을 project spec 초안과 roadmap으로 확장해줘. 구현은 아직 하지 말고, in scope / out of scope / done criteria / initial chunks만 정리해줘.
```

### 2. Design Critic 호출
UI가 필요하고, 내가 명시적으로 요청했을 때만:

```text
이제 Design Critic 관점으로 전환해줘. 이 프로젝트의 UI 방향을 coherence, originality, craft, functionality 기준으로 정리하고, generic AI 스타일을 피하기 위한 금지 패턴도 써줘. 필요하면 2~3개의 방향안을 제시해줘.
```

### 3. Sprint Contract 생성
구현 직전:

```text
Generator와 Evaluator 관점으로 이번 sprint contract를 먼저 작성해줘. 이번 턴에는 한 가지 기능만 구현하도록 잡고, scope, non-goals, done criteria, feature test criteria, integration test criteria, security concerns를 문서 형태로 합의한 뒤에만 구현을 시작해줘.
```

### 4. Generator 구현
실제 구현 단계:

```text
합의된 sprint contract 기준으로 이번 턴의 한 가지 기능만 구현해줘. 다른 기능으로 범위를 넓히지 말고, 구현 후에는 self-check와 known gaps를 남겨줘.
```

### 5. Evaluator 검증
검증 단계:

```text
이제 Evaluator 관점으로 방금 구현한 기능을 검토해줘. feature test와 integration test를 분리해서 contract의 각 항목을 PASS or FAIL로 평가하고, FAIL이면 재현 경로와 수정 방향을 남겨줘. 아직 해결되지 않은 항목은 승인하지 마.
```

### 6. Security Review
민감 데이터나 외부 연동이 있으면:

```text
Security Reviewer 관점으로 이 chunk를 검토해줘. 민감 정보, 로그 노출, 입력 검증, 권한 경계를 중심으로 보고, 기준 미달이면 CHANGES_REQUESTED 또는 BLOCKED로 명확히 남겨줘.
```

### 7. Planner Wrap-up
한 chunk가 끝났을 때:

```text
Planner 관점으로 tracker와 다음 chunk를 갱신해줘. 이번 sprint에서 완료된 것, 남은 리스크, 다음 우선순위를 문서에 반영해줘.
```

## Prompting Guidance
- 처음부터 모든 기능을 한 번에 만들라고 하지 않는다.
- 항상 한 번에 한 기능만 구현하게 한다.
- spec 없는 구현보다 contract 있는 구현을 우선한다.
- 구현 요청과 검증 요청을 분리한다.
- UI 작업은 사용자가 요청했을 때만 시작하게 한다.
- 긴 작업에서는 handoff artifact를 남기게 한다.
- 모델이 충분히 잘하는 쉬운 작업에서는 harness를 가볍게 유지한다.
- 어려운 작업, 긴 작업, 품질 기준이 높은 작업에서는 evaluator를 유지한다.

## Notes
- `harness/scripts/`의 cleanup/checkpoint 스크립트는 그대로 포함했다.
- `harness/runtime/`는 실행 중 생기는 artifact 자리이며 source of truth가 아니다.
- 실제 프로젝트 구현 코드는 각 `apps/<project-name>/src/`와 `tests/` 아래에 둔다.
- 이 템플릿은 Codex 기준으로 진입점을 정리했다.
