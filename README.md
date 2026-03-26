# Ouroboros 마스터 가이드

**"프롬프팅을 멈추고, 사양을 정의하라." -- Specification-First AI Development**

---

## 이 가이드는 무엇인가?

이 가이드는 [Ouroboros](https://github.com/Q00/ouroboros)의 공식 교육 자료입니다. Ouroboros는 Claude Code 플러그인으로 동작하는 **사양 우선 AI 개발 워크플로우 엔진**으로, 모호한 아이디어를 명확한 사양으로 변환하고, AI가 실행한 결과를 검증하며, 수렴할 때까지 진화적으로 반복합니다.

대부분의 AI 코딩이 실패하는 이유는 **출력(output)이 아니라 입력(input)** 때문입니다. Ouroboros는 코드를 작성하기 전에 숨겨진 가정을 드러내어 이 문제를 해결합니다.

이 가이드는 Ouroboros의 17개 스킬, 21개 에이전트(핵심 9 + 지원 12), 22개 MCP 도구를 체계적으로 안내합니다.

> **가이드 버전**: Ouroboros v0.26.x 기준 | **최종 업데이트**: 2026-03-26

---

## 대상 독자

| 수준 | 대상 | 이 가이드에서 시작할 곳 |
|------|------|------------------------|
| **초급** | Ouroboros를 처음 접하는 개발자/PM | [시작하기](categories/getting-started.md) → [학습 경로](01-learning-paths.md) |
| **중급** | 기본 워크플로우를 이해하고 실전에 적용하려는 사용자 | [요구사항 정의](categories/requirements.md) → [개발 실행](categories/development.md) |
| **고급** | 진화적 루프, 합의 평가, 비용 최적화를 활용하려는 사용자 | [진화와 개선](categories/evolution.md) → [용어 사전](02-glossary.md) |

---

## Ouroboros란?

Ouroboros는 세 가지 철학적 기둥 위에 세워져 있습니다.

### 1. 소크라테스적 명확성 (Socratic Clarity)

> "당신이 가정하고 있는 것은 무엇인가?"

모호함 점수(Ambiguity Score)가 **0.2 이하**가 될 때까지 질문합니다. 80% 이상의 가중 명확성에 도달해야만 코드 작성을 시작할 수 있습니다.

### 2. 존재론적 정밀성 (Ontological Precision)

> "이것의 본질은 무엇인가?"

증상이 아니라 **근본 원인**을 해결합니다. 네 가지 근본 질문(본질, 근본 원인, 전제 조건, 숨겨진 가정)으로 문제의 핵심을 파악합니다.

### 3. 진화적 루프 (Evolutionary Loops)

> "각 반복은 단순한 반복이 아니라 진화이다."

평가 결과가 다음 세대의 입력이 됩니다. 온톨로지 유사도가 **0.95 이상**에 도달할 때까지(최대 30세대) 진화를 반복합니다.

---

## Ouroboros 생태계 전체 구조

Ouroboros는 6개 레이어로 구성된 아키텍처입니다.

![Diagram 1](assets/diagrams/README__diagram_1.svg)

---

## 핵심 워크플로우

Ouroboros의 핵심은 **순환하는 진화적 사이클**입니다.

![Diagram 2](assets/diagrams/README__diagram_2.svg)

**두 개의 수학적 게이트**가 품질을 보장합니다:

| 게이트 | 조건 | 역할 |
|--------|------|------|
| **모호함 게이트** | Ambiguity ≤ 0.2 | "명확하지 않으면 만들지 마라" |
| **수렴 게이트** | Similarity ≥ 0.95 | "안정되지 않으면 멈추지 마라" |

---

## 5개 카테고리 개요

### 1. 시작하기 (Getting Started) -- 6 스킬

| 항목 | 내용 |
|------|------|
| **핵심 설명** | 설치부터 첫 사용까지, Ouroboros를 시작하는 데 필요한 모든 것 |
| **주요 스킬** | `setup`, `welcome`, `tutorial`, `help`, `update`, `cancel` |
| **난이도 범위** | 입문 |
| **언제 사용하나** | Ouroboros를 처음 설치하거나, 도움말이 필요하거나, 실행을 중단할 때 |
| **상세 문서** | [categories/getting-started.md](categories/getting-started.md) |

### 2. 요구사항 정의 (Requirements Definition) -- 4 스킬

| 항목 | 내용 |
|------|------|
| **핵심 설명** | 소크라테스적 인터뷰로 모호한 아이디어를 명확한 사양으로 변환 |
| **주요 스킬** | `interview`, `seed`, `pm`, `brownfield` |
| **난이도 범위** | 초급 ~ 중급 |
| **언제 사용하나** | 새 프로젝트를 시작하거나, 기존 코드에 기능을 추가하거나, PRD를 작성할 때 |
| **상세 문서** | [categories/requirements.md](categories/requirements.md) |

### 3. 개발 실행 (Development Execution) -- 3 스킬

| 항목 | 내용 |
|------|------|
| **핵심 설명** | Seed 사양을 실제 코드로 변환하는 AI 실행 엔진 |
| **주요 스킬** | `run`, `ralph`, `status` |
| **난이도 범위** | 중급 |
| **언제 사용하나** | Seed가 준비된 후 코드를 생성하거나, 자동 반복 실행이 필요할 때 |
| **상세 문서** | [categories/development.md](categories/development.md) |

### 4. 품질 검증 (Quality Verification) -- 2 스킬

| 항목 | 내용 |
|------|------|
| **핵심 설명** | 3단계 평가 파이프라인($0 → Standard → Frontier)으로 결과물 검증 |
| **주요 스킬** | `evaluate`, `qa` |
| **난이도 범위** | 중급 ~ 고급 |
| **언제 사용하나** | 실행 결과를 검증하거나, 빠른 품질 판정이 필요할 때 |
| **상세 문서** | [categories/quality.md](categories/quality.md) |

### 5. 진화와 개선 (Evolution & Improvement) -- 2 스킬

| 항목 | 내용 |
|------|------|
| **핵심 설명** | Wonder/Reflect 사이클로 온톨로지를 진화시키고, 막혔을 때 탈출 |
| **주요 스킬** | `evolve`, `unstuck` |
| **난이도 범위** | 고급 |
| **언제 사용하나** | 평가에서 개선이 필요할 때, 정체 상태에 빠졌을 때 |
| **상세 문서** | [categories/evolution.md](categories/evolution.md) |

---

## 명령어 형식

Ouroboros 스킬은 두 가지 형식으로 호출할 수 있습니다:

| 형식 | 예시 | 사용 환경 |
|------|------|-----------|
| `ooo <command>` | `ooo interview` | 개발 모드 (CLAUDE.md 직접 라우팅) |
| `/ouroboros:<command>` | `/ouroboros:interview` | 플러그인 모드 (Claude Code 플러그인) |

> 두 형식은 동일하게 동작합니다. 설치 방법에 따라 편한 형식을 사용하세요.

---

## MCP 모드와 Fallback 모드

Ouroboros 스킬은 **MCP 서버가 없어도** 동작합니다:

| 모드 | 조건 | 특징 |
|------|------|------|
| **MCP 모드** | MCP 서버가 `~/.claude/mcp.json`에 등록됨 | 영구 상태 관리, 비동기 실행, 세션 간 연속성 |
| **Fallback 모드** | MCP 서버 없음 | 대화 컨텍스트 기반, 에이전트 역할 직접 수행 |

> MCP 서버가 없는 상태에서도 interview, seed, unstuck, qa 등 핵심 스킬을 사용할 수 있습니다. `ooo setup`으로 MCP를 설정하면 모든 기능이 활성화됩니다.

---

## 추천 학습 경로 요약

자세한 학습 경로는 [01-learning-paths.md](01-learning-paths.md)를 참고하세요.

| 경로 | 소요 시간 | 핵심 스킬 | 목표 |
|------|-----------|-----------|------|
| **초급** | 30분 | setup → welcome → tutorial → interview → seed | 첫 Seed 사양 생성 |
| **중급** | 2시간 | interview → seed → run → evaluate | 첫 완전 사이클 완료 |
| **고급** | 반나절 | evolve → unstuck → ralph | 진화적 개발 마스터 |

---

## 문서 구조

```
ouroboros-guide/
├── README.md                          ← 현재 문서 (전체 인덱스)
├── 01-learning-paths.md               ← 학습 경로 가이드
├── 02-glossary.md                     ← Ouroboros 용어 사전
├── categories/
│   ├── getting-started.md             ← 시작하기 (6 스킬)
│   ├── requirements.md                ← 요구사항 정의 (4 스킬)
│   ├── development.md                 ← 개발 실행 (3 스킬)
│   ├── quality.md                     ← 품질 검증 (2 스킬)
│   └── evolution.md                   ← 진화와 개선 (2 스킬)
├── assets/
│   └── diagrams/                      ← SVG 다이어그램
└── tools/
    └── render_diagrams.mjs            ← Mermaid → SVG 변환
```

---

## 참고 자료

| 자료 | 링크 |
|------|------|
| Ouroboros GitHub | [github.com/Q00/ouroboros](https://github.com/Q00/ouroboros) |
| PyPI 패키지 | `pip install ouroboros-ai` |
| Claude Code 플러그인 설치 | `claude plugin marketplace add Q00/ouroboros` |
| 아키텍처 문서 | [docs/architecture.md](https://github.com/Q00/ouroboros/blob/main/docs/architecture.md) |

> 이 가이드는 Ouroboros v0.26.x를 기준으로 작성되었습니다. 최신 버전과 차이가 있을 수 있으므로, 각 스킬 설명의 소스 링크를 통해 원본 SKILL.md를 확인하세요.
