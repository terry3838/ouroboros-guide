# Upstream Snapshot — ouroboros

- source repo: `https://github.com/terry3838/ouroboros.git`
- previous synced commit: `896c8cb9e01770c38344faa5e31684a751df5cc3`
- current synced commit: `896c8cb9e01770c38344faa5e31684a751df5cc3`
- sync mode: `no-change`
- impact labels: 일반 변경
- guide repo: `ouroboros-guide`

## 원본 한줄 요약

◯ ─────────── ◯ ◯ ─────────── ◯

## recent upstream commits

- `896c8cb fix(#214): prefer ./mvnw for Maven wrapper projects (#219)`
- `7c2404f fix(install): revert PACKAGE_NAME to base — extras are composed separately`
- `6962fc6 fix(install): add [claude] extra to all ouroboros-ai references (#217)`
- `de0ab03 fix(docs): remove dead Design Documents links + fix nested code block`
- `3faae6a fix(docs): address all review findings on repo guidance`
- `5769933 Remove Discord contact link from issue config`
- `5da2bc6 Tune repo guidance to match maintainer feedback`
- `bf96bf4 Improve issue intake and repository safety guidance`

## top-level structure

- `.claude/`
- `.claude-plugin/`
- `.env.example`
- `.mcp.json`
- `.omc/`
- `.ouroboros/`
- `.pre-commit-config.yaml`
- `.python-version`
- `CHANGELOG.md`
- `CLAUDE.md`
- `CODE_OF_CONDUCT.md`
- `commands/`
- `CONTRIBUTING.md`
- `crates/`
- `docs/`
- `examples/`
- `HANDOFF.md`
- `hooks/`
- `LICENSE`
- `llms-full.txt`

## changed files

- 변경 파일 없음

## README excerpt

```md
<p align="right">
  <strong>English</strong> | <a href="./README.ko.md">한국어</a>
</p>

<p align="center">
  <br/>
  ◯ ─────────── ◯
  <br/><br/>
  <img src="./docs/images/ouroboros.png" width="520" alt="Ouroboros">
  <br/><br/>
  <strong>O U R O B O R O S</strong>
  <br/><br/>
  ◯ ─────────── ◯
  <br/>
</p>


<p align="center">
  <strong>Stop prompting. Start specifying.</strong>
  <br/>
  <sub>Specification-first workflow engine for AI coding agents</sub>
</p>

<p align="center">
  <a href="https://pypi.org/project/ouroboros-ai/"><img src="https://img.shields.io/pypi/v/ouroboros-ai?color=blue" alt="PyPI"></a>
  <a href="https://github.com/Q00/ouroboros/actions/workflows/test.yml"><img src="https://img.shields.io/github/actions/workflow/status/Q00/ouroboros/test.yml?branch=main" alt="Tests"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
</p>

<p align="center">
  <a href="#quick-start">Quick Start</a> ·
  <a href="#why-ouroboros">Why</a> ·
  <a href="#what-you-get">Results</a> ·
  <a href="#the-loop">How It Works</a> ·
  <a href="#commands">Commands</a> ·
  <a href="#from-wonder-to-ontology">Philosophy</a>
</p>

---

> **Security Notice ([REDACTED_PHONE]): LiteLLM PyPI Supply Chain Attack**
>
> LiteLLM versions **1.82.7** and **1.82.8** on PyPI contained a malicious `.pth` file that auto-executes on every Python startup, exfiltrating environment variables, API keys, and credentials to an external server.
>
> **What we did:** Ouroboros now pins LiteLLM to **`<=1.82.6`** (safe versions only) as an optional dependency. LiteLLM is no longer a core dependency — it is only installed when you explicitly opt in via `pip install ouroboros-ai[litellm]`.
>
> **If you previously installed litellm 1.82.7 or 1.82.8**, the malicious `.pth` file may still be on your system even after upgrading. Clean up manually:
>
> ```bash
> # 1. Find and delete the malicious file
> find / -name "litellm_init.pth" 2>/dev/null
>
> # 2. Clean package manager caches
> uv cache clean litellm    # if using uv
> pip cache purge            # if using pip
>
> # 3. Rotate ALL credentials that were on your system during the exposure window
> ```
>
> References: [LiteLLM #24512](https://github.com/BerriAI/litellm/issues/24512) · [Ouroboros #195](https://github.com/Q00/ouroboros/issues/195) · [DSPy mitigation](https://github.com/stanfordnlp/dspy/commit/66e33399bf29)

---

**Turn a vague idea into a verified, working codebase -- with any AI coding agent.**

Ouroboros sits between you and your AI runtime (Claude Code, Codex CLI, or others). It replaces ad-hoc prompting with a structured specification-first workflow: interview, crystallize, execute, evaluate, evolve.

---

## Why Ouroboros?

Most AI coding fails at the **input**, not the output. The bottleneck is not AI capability -- it is human clarity.

| Problem | What Happens | Ouroboros Fix |
|:--------|:-------------|:--------------|
| Vague prompts | AI guesses, you rework | Socratic interview exposes hidden assumptions |
| No spec | Architecture drifts mid-build | Immutable seed spec locks intent before code |
| Manual QA | "Looks good" is not verification | 3-stage automated evaluation gate |

---

## Quick Start

**Install** — one command, everything auto-detected:

```bash
curl -fsSL https://raw.githubusercontent.com/Q00/ouroboros/main/scripts/install.sh | bash
```

**Build** — open your AI coding agent and go:

```
> ooo interview "I want to build a task management CLI"
```

> Works with Claude Code and Codex CLI. The installer detects your runtime, registers the MCP server, and installs skills automatically.

<details>
<summary><strong>Other install methods</strong></summary>

**Claude Code plugin only** (no system package):
```bash
claude plugin marketplace add Q00/ouroboros && claude plugin install ouroboros@ouroboros
```
Then run `ooo setup` inside a Claude Code session.

**pip / uv / pipx**:
```bash
pip install ouroboros-ai                # base
pip install ouroboros-ai[claude]        # + Claude Code deps
pip install ouroboros-ai[all]           # everything
ouroboros setup                         # configure runtime
```

See runtime guides: [Claude Code](./docs/runtime-guides/claude-code.md) · [Codex CLI](./docs/runtime-guides/codex.md)

</details>

> **Python >= 3.12 required.** See [pyproject.toml](./pyproject.toml) for the full dependency list.
```
