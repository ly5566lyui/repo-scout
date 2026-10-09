# repo-scout · Repository Scout

> 🌐 [中文](README.zh-CN.md) · **English**

**Research any public GitHub repository in minutes — activity, architecture, and AI-ready code material — using only free online services.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-blueviolet.svg)](https://agentskills.io)

> This skill follows the open **Agent Skills** standard ([agentskills.io](https://agentskills.io)). It is a portable `SKILL.md` folder that works with Claude Code, Cursor, Codex, Gemini CLI, OpenCode, VS Code / Copilot, Windsurf, Cline, Roo Code, and 25+ other agents — it is not tied to any single platform.

## What it solves

Facing an unfamiliar repository, the usual questions are:

- Is this project alive? Is it well maintained?
- How is it architected? Where is the core code?
- The repo is large — how do I feed its material to an LLM without blowing the context window?

repo-scout turns these into a repeatable process instead of an ad-hoc scramble. Everything runs through web services: **no local install, no API key.** The core trick is to replace `github.com` in the repository URL with the corresponding service's domain.

## Why repo-scout

- **Agent-agnostic.** One `SKILL.md`, driven by a standard 25+ agents already implement — install once, use anywhere.
- **Zero setup.** No CLIs to install, no tokens to manage, nothing to sign up for.
- **Resilient by design.** Each stage has a documented fallback chain, and there's a browserless data path via the GitHub REST API, so the flow survives a flaky or rate-limited service.
- **Context-aware.** For monorepos it extracts only the core directories and reports token counts, so you don't blow your context window.
- **Predictable output.** Always the same five sections, so reports are easy to compare and automate.

## The five stages

| Stage | What it does | Tool |
| --- | --- | --- |
| 1. Quick assessment | Star-history curve for activity; check for an existing knowledge base | star-history, DeepWiki |
| 2. Architecture | Generate an architecture diagram; identify the most-depended-on core directories | gitdiagram |
| 3. Material extraction | Full / sub-path / filtered extraction with token control | gitingest, repomix, uithub |
| 4. Targeted deep read | Verify raw code online; settle open questions | GitHub1s |
| 5. Persist (optional) | Turn the repo into a persistent MCP knowledge base | git-mcp |

If DeepWiki already hosts a wiki for the repo, the flow short-circuits and skips stages 2 and 3.

## Worked example

Ask: *"Research this repo: https://github.com/pallets/flask — is it active, how is it architected, and where's the core code?"*

The skill produces a report like:

```markdown
## 1. Positioning
Flask — Pallets' lightweight WSGI web framework for Python. Dependencies: werkzeug, jinja2, click.

## 2. Architecture highlights
Three layers: `src/flask/sansio/` (IO-free core, reused by Quart) → `src/flask/*.py`
(WSGI runtime) → `src/flask/json/`. The `Scaffold → App → Flask` inheritance chain
carries routing, config, and the WSGI call path.

## 3. Key directories
- src/flask/app.py        — Flask class, request dispatch, WSGI __call__
- src/flask/sansio/       — shared, IO-free core
- src/flask/ctx.py        — app / request contexts

## 4. Activity & risk
Last commit within 24h; latest release ~7 months ago; only 4 open issues.
Active but slow-moving; bus factor leans on one maintainer.

## 5. Suggested next actions
Pin to the latest tag (3.1.3) for production; read docs/design.rst then the four key files.
```

## Install

### Option 1: One command (recommended — auto-detects your agents)

Use the open skills CLI; it discovers the Cursor, Claude Code, Codex, OpenCode, etc. installed on your machine:

```bash
# Install from this GitHub repo
npx skills add ly5566lyui/repo-scout

# Or install globally to every detected agent
npx skills add ly5566lyui/repo-scout -g --all

# Or target specific agents
npx skills add ly5566lyui/repo-scout -a claude-code -a cursor
```

### Option 2: Manual copy

Copy the `skills/repo-scout` folder (containing `SKILL.md`) into the skills directory of your target agent:

| Agent | Project path | Global path |
| --- | --- | --- |
| Cross-client standard | `.agents/skills/` | `~/.agents/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Cursor | `.cursor/skills/` | `~/.cursor/skills/` |
| Codex | `.codex/skills/` | `~/.codex/skills/` |
| Gemini CLI | `.gemini/skills/` | `~/.gemini/skills/` |
| OpenCode | `.opencode/skills/` | `~/.config/opencode/skills/` |
| VS Code / GitHub Copilot | `.github/skills/` | `~/.copilot/skills/` |
| Windsurf | `.windsurf/skills/` | — |
| Cline | `.cline/skills/` | `~/.cline/skills/` |
| Roo Code | `.roo/skills/` | `~/.roo/skills/` |
| Trae | `.trae/skills/` | — |
| Amp | `.agents/skills/` | `~/.config/agents/skills/` |

> The directory name must match the `name` field in `SKILL.md` (here: `repo-scout`). The cross-client standard path `.agents/skills/` is recognised by Codex, Cursor, Amp, OpenCode and others — install once, use everywhere.

### Option 3: Agents without native skill support

If an agent (some Windsurf / Copilot versions, etc.) does not support skills natively, paste the body of `SKILL.md` into its rules / instructions file (e.g. `AGENTS.md`, `.windsurfrules`, `.github/copilot-instructions.md`, `GEMINI.md`), or just paste the frontmatter `description` plus the five-stage workflow.

## Usage

The skill triggers automatically based on its `description`. Just say something like:

- "Research this repo: https://github.com/xxx/yyy"
- "Is this open-source project any good? Assess its activity and maintenance risk."
- "Help me quickly understand this repo's architecture and key code."
- "Turn repo X into material suitable for feeding to an LLM."

You can also invoke it explicitly: type `/repo-scout` in Claude Code / Cursor, or `$repo-scout` in Codex.

It always produces five sections:

1. Positioning
2. Architecture highlights
3. Key directories
4. Activity & risk
5. Suggested next actions

## Supported tools

| Tool | Purpose | URL rule |
| --- | --- | --- |
| [star-history](https://star-history.com) | Star-history curve for activity | Visit directly (redirects to www) |
| [DeepWiki](https://deepwiki.com) | Ready-made repo wiki + Q&A | Visit directly |
| [gitdiagram](https://gitdiagram.com) | Interactive architecture diagram (Mermaid/PNG) | github.com → gitdiagram.com |
| [gitingest](https://gitingest.com) | Structured text extraction + token stats | github.com → gitingest.com |
| [GitHub1s](https://github1s.com) | Browse code in an online VS Code | github.com → github1s.com |
| [uithub](https://uithub.com) | Leaner LLM-ready output | github → uithub |
| [Repomix](https://repomix.com) | Filtered extraction + token stats | Paste URL on the web app |
| [git-mcp](https://gitmcp.io) | Persistent MCP knowledge base | github.com → gitmcp.io |

For detailed usage, limitations and fallback chains, see [`references/tools.md`](references/tools.md); for monorepo extraction strategy, see [`references/extraction.md`](references/extraction.md).

> The URL rules differ per tool (uithub swaps the first letter; gitdiagram/gitingest swap `hub`; GitHub1s swaps the whole domain). The skill checks each one rather than applying a single rule.

## Evaluation

`evals/evals.json` holds behaviour evaluation cases (what conclusions the skill should produce). Run a "with-skill vs without-skill" comparison in any skill-eval-capable workflow to confirm the skill improves output quality. For trigger accuracy (whether the description matches correctly), test it systematically as described in the [Agent Skills description-optimization guide](https://agentskills.io/skill-creation/optimizing-descriptions).

## FAQ & troubleshooting

**A service returns nothing / seems broken.**
That's expected sometimes — these are free services with anonymous rate limits, and some render client-side. The skill's fallback chains handle it; when nothing else works it falls back to the GitHub REST API (`api.github.com`), which needs no auth for public repos.

**Do I need a browser?**
Not strictly. A real browser gives the best results for the JS-rendered services, but an agent with only static fetching still works via the API fallback (stages 2–3 degrade gracefully).

**Does it work on private repos?**
No — public repositories only. Private repos and login-gated pages are explicitly out of scope.

**The architecture diagram looks wrong.**
gitdiagram's diagram is LLM-generated and can be inaccurate. Stage 4 (reading the raw code via GitHub1s) is the source of truth — treat the diagram as a hint.

## Roadmap

- [ ] Optional GitHub-token mode for higher rate limits and private-repo support.
- [ ] A `--json` output contract for machine consumption.
- [ ] More tool adapters and additional fallback sources.

## Known limitations

- Only **public repositories**; private repos and pages that require login are out of scope.
- Free services have anonymous rate limits and may throttle; the skill's fallback chains cover this.
- gitdiagram's diagram is LLM-generated and may be inaccurate — treat stage 4's raw-code verification as the source of truth.
- Third-party services may change or disappear; the fallbacks mitigate this, but it's worth periodically re-checking the links.

## Contributing

Contributions are welcome — see the repo's [CONTRIBUTING.md](https://github.com/ly5566lyui/repo-scout/blob/main/CONTRIBUTING.md).

## License

[MIT](LICENSE)

## Acknowledgements

star-history, DeepWiki (open-source DeepWiki-Open), gitdiagram, gitingest, GitHub1s, uithub, Repomix and git-mcp are open-source or free online projects by their respective authors, most under the MIT license. This skill only calls their public web services and does not redistribute their source code.