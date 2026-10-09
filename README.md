# repo-scout

> 🌐 [中文](README.zh-CN.md) · **English**

**Agent Skills for understanding unfamiliar GitHub repositories — fast.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-blueviolet.svg)](https://agentskills.io)
[![GitHub stars](https://img.shields.io/github/stars/ly5566lyui/repo-scout?style=social)](https://github.com/ly5566lyui/repo-scout/stargazers)

## About

This repository publishes AI skills in the open [Agent Skills](https://agentskills.io) format — a portable `SKILL.md` folder that works across 25+ agents (Claude Code, Cursor, Codex, Gemini CLI, OpenCode, VS Code / Copilot, Windsurf, Cline, Roo Code, …) with no vendor lock-in.

The headline skill, **[repo-scout](skills/repo-scout/README.md)**, turns any public GitHub repository into a structured research report — activity & maintenance risk, an architecture diagram, and AI-friendly code material — using only free online services, with **no local install and no API key**.

## Skills

| Skill | What it does |
| --- | --- |
| [**repo-scout**](skills/repo-scout/README.md) · 仓库速览 | A 5-stage pipeline (assess → diagram → extract → deep-read → persist) that researches a repository and returns a fixed five-section report. [中文说明](skills/repo-scout/README.zh-CN.md) |

## Features

- 🧩 **Agent-agnostic** — one `SKILL.md`, driven by an open standard that 25+ agents already implement.
- 🆓 **Zero setup** — pure web services; no local tools, no API keys, no sign-up.
- 🛡️ **Resilient** — every stage ships with a documented fallback chain, so the flow never dead-ends.
- 🧠 **Context-aware** — token-budget discipline keeps huge monorepos from blowing your context window.
- 📋 **Predictable output** — always the same five sections, so results are easy to compare and automate.

## Installation

```bash
# Auto-detect the agents installed on your machine and install
npx skills add ly5566lyui/repo-scout

# Install globally to every detected agent
npx skills add ly5566lyui/repo-scout -g --all

# Target specific agents
npx skills add ly5566lyui/repo-scout -a claude-code -a cursor
```

Prefer a manual copy? Drop `skills/repo-scout/` into your agent's skills directory — the [skill README](skills/repo-scout/README.md#install) lists the exact path for each agent (`.agents/skills/`, `.claude/skills/`, `.cursor/skills/`, `~/.config/opencode/skills/`, …).

## Usage

Once installed, just talk to your agent:

- "Research this repo: https://github.com/pallets/flask"
- "Is this open-source project any good? Assess its activity and maintenance risk."
- "Help me quickly understand this repo's architecture and key code."
- "Turn repo X into material suitable for feeding to an LLM."

You'll get a report with a fixed shape:

```
1. Positioning          2. Architecture highlights     3. Key directories
4. Activity & risk      5. Suggested next actions
```

## Repository layout

```
.
├── README.md            # This file (English)
├── README.zh-CN.md      # 中文说明
├── CONTRIBUTING.md      # Contribution guide
├── CONTRIBUTING.zh-CN.md
├── LICENSE              # MIT
└── skills/
    └── repo-scout/
        ├── SKILL.md          # The skill: frontmatter + 5-stage workflow + output contract
        ├── README.md         # Full skill guide (English)
        ├── README.zh-CN.md   # 技能完整说明（中文）
        ├── LICENSE           # Bundled so the skill is self-contained
        ├── references/
        │   ├── tools.md      # Tool usage, URL rules, limits, fallback chains
        │   └── extraction.md # Monorepo extraction & token control
        └── evals/
            └── evals.json    # Behaviour evaluation cases
```

## Documentation

| Topic | Where |
| --- | --- |
| What the skill does, how to use it | [skills/repo-scout/README.md](skills/repo-scout/README.md) |
| Tool details, URL rules & fallbacks | [skills/repo-scout/references/tools.md](skills/repo-scout/references/tools.md) |
| Monorepo extraction strategy | [skills/repo-scout/references/extraction.md](skills/repo-scout/references/extraction.md) |
| The Agent Skills standard | [agentskills.io](https://agentskills.io) |

## Contributing

Ideas, bug reports and pull requests are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md), including how to add a **new skill** to this repository.

## License

Released under the [MIT license](LICENSE).

## Acknowledgments

`repo-scout` orchestrates the public web services **star-history, DeepWiki, gitdiagram, gitingest, GitHub1s, uithub, Repomix and git-mcp** — open-source or free projects by their respective authors, most under MIT. This repository only calls their public services and does not redistribute their source code.