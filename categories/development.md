# 개발 실행 (Development Execution)

## 카테고리 소개

개발 실행 카테고리는 Ouroboros 워크플로우의 핵심 변환 단계를 담당합니다. `ooo seed`로 정의된 Seed YAML 명세를 실제 동작하는 코드로 전환하는 과정이 바로 이 카테고리에서 이루어집니다. 명세가 아무리 정밀하게 작성되었더라도 실행 단계에서의 판단과 조율이 뒷받침되지 않으면 원하는 결과를 얻기 어렵습니다. 이 카테고리는 그 간극을 메우기 위해 설계되었습니다.

PAL(Prompt-Adaptive Load) 라우터는 이 카테고리의 핵심 지능입니다. Seed의 복잡도를 자동으로 측정하고 Frugal, Standard, Frontier 세 가지 모델 티어 중 가장 적합한 것을 선택합니다. 단순한 작업에 고비용 모델을 낭비하거나, 복잡한 작업에 저사양 모델을 투입하는 실수를 방지하여 비용과 품질 사이의 균형을 자동으로 최적화합니다.

실행 방식은 Double Diamond 모델을 따릅니다. Discover(탐색) → Define(정의) → Design(설계) → Deliver(전달)의 네 단계를 거치며 AC(Acceptance Criteria)를 MECE 원칙에 따라 원자적 태스크로 분해합니다. 각 태스크는 독립적으로 검증 가능하며, 실패 시 자동 에스컬레이션을 통해 상위 모델 티어로 재시도합니다.

`ooo ralph`는 이 실행 루프를 완전 자동화합니다. "The boulder never stops"라는 철학 아래, 실행 → QA 검증 → 수정의 사이클을 검증이 통과할 때까지 반복합니다. `ooo status`는 이 과정 전체를 모니터링하며 현재 세션의 진행 상태, 드리프트 측정치, AC 완료율을 한눈에 보여줍니다. 세 스킬이 함께 작동할 때 개발 실행 카테고리는 가장 강력한 효과를 발휘합니다.

---

## 이 카테고리의 스킬 한눈에 보기

| 스킬 | 명령어 | 핵심 기능 | 난이도 |
|------|--------|-----------|--------|
| **run** | `ooo run` | Seed를 Double Diamond로 실행, PAL 라우터 자동 선택 | 중급 |
| **ralph** | `ooo ralph` | 실행→QA→수정 자동 반복 루프, 검증 통과까지 지속 | 중급 |
| **status** | `ooo status` | 세션 상태·AC 진행률·드리프트 점수 조회 | 초급 |

---

## 추천 실행 순서

![Diagram 1](../assets/diagrams/categories__development__diagram_1.svg)

권장 실행 순서:

1. **`ooo seed`** (요구사항 정의 카테고리) — 실행 전 반드시 Seed가 생성되어 있어야 합니다.
2. **`ooo run [session_id]`** — Seed를 코드로 변환합니다. 복잡도가 높다면 시간이 걸릴 수 있습니다.
3. **`ooo status [session_id]`** — 실행 진행 상황과 AC 완료율을 확인합니다.
4. **`ooo evaluate`** (품질 검증 카테고리) — 생성된 코드를 평가합니다.
5. **`ooo ralph "task"`** — 검증 실패 시 자동 반복 루프로 전환하여 통과까지 반복합니다.

---

## 관련 카테고리

- **요구사항 정의**: `ooo seed`로 Seed를 생성해야 실행이 가능합니다. 명세 없이는 실행할 수 없습니다.
- **품질 검증**: 실행 후 `ooo evaluate` 또는 `ooo qa`로 생성된 코드의 품질을 검증합니다.
- **진화와 개선**: 검증 실패 시 `ooo evolve`로 Seed를 개선하고 재실행하는 진화 루프를 구성합니다.

---

## 스킬 상세

### run (실행)

**Seed를 Double Diamond 프로세스로 실행하여 코드를 생성합니다.**

#### 명령어

```
ooo run [session_id]
/ouroboros:run [session_id]
```

`session_id`를 생략하면 현재 활성 세션을 사용합니다.

#### 작동 방식

`ooo run`은 Seed YAML 명세를 입력받아 실제 코드로 변환하는 핵심 실행 스킬입니다. 내부적으로 두 가지 주요 메커니즘이 협력합니다.

**PAL 라우터 (Prompt-Adaptive Load Router)**

실행 전 Seed의 복잡도를 자동 측정하여 세 가지 모델 티어 중 하나를 선택합니다.

| 티어 | 복잡도 범위 | 상대 비용 | 특징 |
|------|-------------|-----------|------|
| Frugal | < 0.4 | 1x | 단순 CRUD, 명확한 명세 |
| Standard | 0.4 ~ 0.7 | 10x | 중간 복잡도, 일반 비즈니스 로직 |
| Frontier | ≥ 0.7 | 30x | 고복잡도, 설계 판단 필요 |

실패 시 자동 에스컬레이션(Frugal → Standard → Frontier), 성공 시 자동 다운그레이드가 이루어집니다.

**Double Diamond 실행 모델**

AC(Acceptance Criteria)를 MECE 원칙에 따라 원자적 태스크로 분해한 뒤 네 단계를 순서대로 진행합니다.

- **Discover**: 요구사항 탐색, 모호한 부분 식별
- **Define**: 해결할 문제 범위 확정
- **Design**: 구현 설계 및 아키텍처 결정
- **Deliver**: 실제 코드 생성 및 제공

#### MCP 모드 (MCP 연결 시)

MCP가 연결된 환경에서는 백그라운드 비동기 실행을 지원합니다.

```
ouroboros_start_execute_seed  →  job_id 즉시 반환
ouroboros_job_wait(job_id)    →  완료 대기
ouroboros_execute_seed        →  동기 실행 (직접 결과 반환)
```

`ouroboros_start_execute_seed`는 즉시 `job_id`를 반환하므로 장시간 실행 작업에서도 인터페이스가 차단되지 않습니다.

#### 폴백 모드 (MCP 미연결 시)

MCP가 없는 환경에서는 `code-executor` 에이전트를 직접 로드하여 순차 실행합니다. 기능은 동일하나 백그라운드 실행이 지원되지 않습니다.

```
# 스킬 파일 위치
skills/run/SKILL.md
src/ouroboros/agents/code-executor.md
```

#### 출력

Seed 명세를 구현하는 생성된 코드. AC별 완료 상태가 함께 제공됩니다.

- **난이도**: 중급
- **소요 시간**: 가변 (복잡도에 따라 수 분 ~ 수십 분)

---

### ralph (자동 반복 루프)

**검증이 통과될 때까지 실행→QA→수정 루프를 자동으로 반복합니다.**

#### 명령어

```
ooo ralph "task"
/ouroboros:ralph "task"
```

#### 작동 방식

`ooo ralph`는 Ouroboros의 자율 실행 루프입니다. "The boulder never stops"라는 철학 아래, QA 판정이 PASS가 될 때까지 또는 최대 반복 횟수에 도달할 때까지 멈추지 않습니다.

**루프 구조**

```
Execute → QA 검증 → [PASS] → 완료
                  → [FAIL/REVISE] → Fix → Execute (반복)
```

각 반복 사이클에서 ralph는 다음을 수행합니다.

1. `ooo run`에 해당하는 실행 단계로 코드를 생성 또는 수정합니다.
2. `ooo qa`에 해당하는 QA 판정을 실행합니다.
3. FAIL 또는 REVISE 판정이 나오면 실패 원인을 분석하고 수정 계획을 수립합니다.
4. `ooo evolve`와 연계하여 Seed 자체를 개선한 뒤 재실행할 수 있습니다.

**진화 연계 실행**

ralph는 단순 재실행에 그치지 않고 `evolve_step(execute=true)`를 호출하여 실행과 진화를 동시에 수행할 수 있습니다. 반복이 거듭될수록 Seed 명세 자체가 개선되어 더 나은 코드가 생성됩니다.

**세션 복원**

ralph는 무상태(stateless) 설계를 따릅니다. EventStore에 기록된 이벤트를 재구성하여 세션이 중단되었다가 재개되더라도 이전 상태를 정확히 복원합니다.

#### MCP 모드 (MCP 연결 시)

```
ouroboros_execute_seed   →  코드 실행
ouroboros_qa             →  품질 판정
ouroboros_evolve_step    →  진화 단계 (execute=true로 실행 병행)
```

#### 폴백 모드 (MCP 미연결 시)

대화 컨텍스트 내에서 execute → evaluate → fix 루프를 순차적으로 수행합니다. MCP 모드와 동일한 논리를 따르지만 백그라운드 실행 및 EventStore 복원은 지원되지 않습니다.

```
# 스킬 파일 위치
skills/ralph/SKILL.md
```

#### 출력

검증을 통과한 완성된 코드. 최종 QA 리포트와 반복 횟수가 함께 제공됩니다.

- **난이도**: 중급
- **소요 시간**: 가변 (반복 횟수에 따라 결정)

---

### status (세션 상태)

**현재 세션의 진행 상태, AC 완료율, 드리프트 점수를 조회합니다.**

#### 명령어

```
ooo status [session_id]
/ouroboros:status [session_id]

# 드리프트 측정 별칭
ooo drift [session_id]
```

`session_id`를 생략하면 현재 활성 세션의 상태를 조회합니다.

#### 작동 방식

`ooo status`는 실행 중이거나 완료된 세션의 현재 상태를 종합적으로 보고합니다. 장시간 실행되는 `ooo run` 또는 `ooo ralph` 작업의 진행 상황을 확인하거나, 이전 세션의 드리프트 수준을 측정할 때 사용합니다.

**보고 항목**

| 항목 | 설명 |
|------|------|
| 현재 세대 (generation) | 진화 사이클 횟수 |
| AC 진행률 | 완료된 Acceptance Criteria 비율 |
| 드리프트 점수 | 구현과 명세 간의 편차 (0.0 ~ 1.0) |
| 세션 상태 | running / completed / failed |

**드리프트 점수 해석**

드리프트 점수는 현재 코드가 원래 Seed 명세에서 얼마나 벗어났는지를 나타냅니다. 점수가 높을수록 `ooo evolve`를 통한 명세 재정렬이 권장됩니다.

#### MCP 모드 (MCP 연결 시)

```
ouroboros_session_status    →  세션 전체 상태 조회
ouroboros_measure_drift     →  드리프트 점수 계산
```

#### 폴백 모드 (MCP 미연결 시)

MCP가 없는 환경에서는 현재 대화 컨텍스트 내에서 추적 가능한 진행 상황만 표시됩니다. EventStore 기반의 세대 정보나 정확한 드리프트 점수는 MCP 연결 시에만 제공됩니다.

```
# 스킬 파일 위치
skills/status/SKILL.md
```

#### 출력

세션 상태 리포트. 현재 세대, AC 완료율, 드리프트 점수, 권장 다음 단계가 포함됩니다.

- **난이도**: 초급
- **소요 시간**: 약 1분
