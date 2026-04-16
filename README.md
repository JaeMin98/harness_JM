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

하지만 당신이 원하는 방식이 `내가 목표만 말하면 Codex가 하네스를 스스로 참조하며 자동으로 진행하는 구조`라면, 처음부터 그렇게 지시할 수도 있다. 핵심은 Codex에게:

- 먼저 하네스 문서를 읽게 하고
- 현재 단계에서 어떤 역할로 움직일지 스스로 선택하게 하고
- 한 번에 한 기능만 구현하게 하고
- 기능별 테스트와 통합 테스트를 구분하게 하고
- 매 단계마다 tracker와 artifact를 갱신하게 하는 것

이다.

### Recommended Autonomous Command
새 프로젝트에서 가장 추천하는 시작 명령은 아래 형태다.

```text
이 레포는 harness_JM 기반 프로젝트다. 먼저 AGENTS.md, harness/core/docs/index.md, harness/core/workflows/pipeline.md, apps/<project-name>/harness/docs/index.md, apps/<project-name>/harness/plans/tracker.md 를 스스로 읽고, 그 규칙에 따라 작업해줘.

내 요청을 먼저 Planner 관점에서 spec과 roadmap으로 정리하고, 그 다음에는 필요한 역할을 스스로 전환해가며 진행해줘.

- 구현은 항상 한 번에 한 기능씩만 진행해
- 구현 전에는 Generator/Evaluator 계약을 먼저 잡아
- 테스트는 feature test와 integration test를 구분해
- UI/디자인 작업은 내가 명시적으로 요청한 경우에만 수행해
- 각 단계가 끝날 때마다 tracker와 handoff artifact를 갱신해
- 범위가 애매하면 멋대로 크게 확장하지 말고 현재 기능 하나 기준으로 좁혀서 진행해

이번 요청: <여기에 실제 목표를 적기>
```

이 명령의 의도는 “방법은 하네스가 정하고, 나는 목표만 준다”를 Codex에게 분명히 전달하는 것이다.

### One-Shot Bootstrap Command
프로젝트명을 예를 들어 `my_project`라고 줄 때, 템플릿 clone, 프로젝트 폴더 준비, 하네스 문서 초기화 방향까지 한 번에 지시하고 싶다면 아래 프롬프트를 그대로 쓰면 된다.

```text
내 새 프로젝트 이름은 `my_project`다.

`harness_JM` 템플릿을 기반으로 새 프로젝트를 시작해줘. 아래 순서까지 한 번에 진행해:

1. `harness_JM` 레포를 clone해서 새 작업 디렉토리를 만든다
2. 새 레포 또는 새 작업 폴더의 이름을 `my_project` 기준으로 맞춘다
3. `apps/sample-project/`를 복사해서 `apps/my_project/`를 만든다
4. `apps/my_project/` 안의 project-level harness 문서에서 placeholder를 `my_project` 기준으로 초기화한다
5. `AGENTS.md`, `harness/core/docs/index.md`, `harness/core/workflows/pipeline.md`, `apps/my_project/harness/docs/index.md`, `apps/my_project/harness/plans/tracker.md`를 스스로 읽고 작업 준비를 끝낸다
6. 내 아래 요청을 Planner 관점에서 spec, roadmap, initial feature list로 확장한다
7. 그 뒤에는 하네스 규칙에 따라 필요한 역할을 스스로 전환하며 자동 진행한다

작업 규칙:
- 구현은 항상 한 번에 한 기능씩만 진행해
- 구현 전에는 Generator/Evaluator contract를 먼저 작성해
- feature test와 integration test를 분리해
- UI/디자인 작업은 내가 명시적으로 요청할 때만 진행해
- 각 단계가 끝날 때마다 tracker와 handoff artifact를 갱신해
- 범위가 크면 멋대로 한 번에 구현하지 말고 첫 기능 하나로 쪼개서 시작해

이번 프로젝트 목표:
<여기에 실제 목표를 적기>
```

이 프롬프트는 새 프로젝트를 부트스트랩할 때 가장 강력한 시작점이다.

### One-Shot Bootstrap Command With Repository Creation
새 GitHub 레포 이름까지 `my_project`로 맞추고 싶다면 아래처럼 더 구체적으로 시킬 수 있다.

```text
내 새 프로젝트 이름은 `my_project`다.

`harness_JM`을 clone해서 `my_project` 작업 디렉토리를 만들고, `apps/sample-project/`를 `apps/my_project/`로 복사해 초기화해줘. 가능하면 git 원격도 새 프로젝트 이름 기준으로 맞출 수 있게 준비해줘.

그 다음에는 하네스 문서를 스스로 읽고, Planner -> Generator -> Security Reviewer -> Evaluator 흐름을 필요에 따라 적용하면서 자동으로 진행해줘.

규칙은 아래를 반드시 따라:
- 구현은 한 번에 한 기능만
- 구현 전 contract 작성
- feature test / integration test 분리
- UI는 내가 요청할 때만
- tracker와 handoff artifact 계속 갱신

이번 프로젝트 목표:
<여기에 실제 목표를 적기>
```

이 버전은 로컬 부트스트랩뿐 아니라 “새 프로젝트 이름에 맞춘 전체 시작 흐름”까지 함께 암시한다.

### Short Autonomous Command
더 짧게 쓰고 싶으면 아래처럼 해도 된다.

```text
이 프로젝트는 harness_JM 규칙대로 진행해줘. 관련 harness 문서를 스스로 읽고, Planner -> Generator -> Security Reviewer -> Evaluator 흐름을 필요에 따라 적용해. 한 번에 한 기능만 구현하고, feature test와 integration test를 분리하고, UI는 내가 요청할 때만 다뤄줘. tracker도 계속 갱신해. 이번 목표는 <실제 목표>.
```

### Best Practice For Autonomous Use
- 목표는 짧게 주고, 방법은 하네스에 맡긴다.
- 처음부터 여러 기능을 한 번에 요구하지 않는다.
- `이번 기능 하나` 수준으로 요청할수록 자동화가 안정적이다.
- UI가 필요 없으면 UI를 언급하지 않는다.
- 검증까지 원하면 `테스트와 평가까지 끝내줘`를 함께 적는다.
- 장기 작업이면 `각 단계 결과를 tracker에 반영해줘`를 같이 적는다.

### Good Command Examples
```text
이 프로젝트는 harness_JM 규칙대로 자동 진행해줘. 하네스 문서를 먼저 읽고, 이번에는 사용자 로그인 기능 하나만 설계부터 구현, feature test, integration test, tracker 갱신까지 진행해줘. UI 작업은 하지 마.
```

```text
harness_JM 방식으로 진행해줘. 이번 목표는 결제 내역 조회 기능 하나를 추가하는 것이다. 필요한 역할을 스스로 전환하면서 contract 작성, 구현, 검증, tracker 갱신까지 해줘.
```

```text
먼저 harness 문서를 읽고 자동으로 작업해줘. 이번에는 관리자 대시보드 UI가 필요하니 Design Critic도 포함해줘. 하지만 이번 턴에서는 대시보드의 통계 카드 표시 기능 하나만 진행해줘.
```

### Bad Command Examples
아래 같은 요청은 자동화 품질을 떨어뜨릴 수 있다.

```text
전체 서비스 다 만들어줘.
```

이유:
- 범위가 너무 넓다.
- 한 기능 단위 진행 원칙이 깨진다.

```text
로그인, 결제, 관리자 페이지, 알림 시스템까지 한 번에 다 해줘.
```

이유:
- contract가 여러 기능에 걸쳐 퍼진다.
- 테스트와 handoff가 흐려진다.

```text
예쁘게 UI도 만들고 백엔드도 만들고 알아서 다 해줘.
```

이유:
- UI가 기본 활성화되지 않는데도 범위가 섞인다.
- 어떤 기능부터 해야 하는지 불분명하다.

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
