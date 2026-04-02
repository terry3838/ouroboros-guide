# Obsidian Vault Map

이 문서는 `ouroboros-guide` 의 repo guide 문서를 Obsidian note pack과 어떻게 대응시키는지 정리한다.

## 출력 모드

- mode: `hybrid`
- repo guide: `ouroboros-guide/`
- repo-local note pack: `obsidian/ouroboros Guide/`
- live vault target: `unset`
- live sync status: `not applied`

## 안전 원칙

- repo-local pack이 정본이다.
- live vault sync는 **의도된 vault target이 명시적으로 확인된 뒤에만** 수행한다.
- default vault나 CLI 반환값은 참고 정보일 뿐, 목적지 확정 근거가 아니다.

## 매핑

| repo guide | note pack |
|---|---|
| `README.md` | `ouroboros Guide/ouroboros Guide - MOC.md` |
| `01-learning-paths.md` | `ouroboros Guide/02 Learning Paths.md` |
| `02-glossary.md` | `ouroboros Guide/03 Glossary.md` |
| `categories/development.md` | `ouroboros Guide/Categories/Development.md` |
| `categories/evolution.md` | `ouroboros Guide/Categories/Evolution.md` |
| `categories/getting-started.md` | `ouroboros Guide/Categories/Getting started.md` |
| `categories/overview.md` | `ouroboros Guide/01 Overview.md` |
| `categories/quality.md` | `ouroboros Guide/Categories/Quality.md` |
| `categories/requirements.md` | `ouroboros Guide/Categories/Requirements.md` |

## note map 의도

- MOC가 frontdoor 역할을 한다.
- Learning Paths / Glossary가 기본 3축이 된다.
- deep dive는 source-backed reading surface로 붙인다.
- References와 Workflows는 길찾기/증거를 붙잡아 둔다.
