# repo-scout · Repository Scout

> 🌐 [中文](README.zh-CN.md) · **English**

Research any public GitHub repository using a set of **free online services** — assess activity and maintenance risk, generate an architecture diagram, extract AI-friendly code material, and produce a structured report, all in one pass.

Everything runs through web services: **no local install, no API key required.** The core trick is to replace `github.com` in the repository URL with the corresponding service's domain.

> This skill follows the open **Agent Skills** standard ([agentskills.io](https://agentskills.io)). It is a portable `SKILL.md` folder that works with Claude Code, Cursor, Codex, Gemini CLI, OpenCode, VS Code / Copilot, Windsurf, Cline, Roo Code, and 25+ other agents — it is not tied to any single platform.

## What it solves

Facing an unfamiliar repository, the usual questions are:

- Is this project alive? Is it well maintained?
- How is it architected? Where is the core code?
- The repo is large — how do I feed its material to an LLM without blowing the context window?

repo-scout breaks these down into a **five-stage pipeline**, using the best free web service for each stage, with built-in fallback chains so the flow never gets stuck:

| Stage | What it does | Tool |
| --- | --- | --- |
| 1. Quick assessment | Star-history curve for activity; check for an existing knowledge base | star-history, DeepWiki |
| 2. Architecture | Generate an architecture diagram; identify the most-depended-on core directories | gitdiagram |
| 3. Material extraction | Full / sub-path / filtered extraction with token control | gitingest, repomix, uithub |
| 4. Targeted deep read | Verify raw code online; settle open questions | GitHub1s |
| 5. Persist (optional) | Turn the repo into a persistent MCP knowledge base | git-mcp |

If DeepWiki already hosts a wiki for the repo, the flow short-circuits and skips stages 2 and 3.

## Directory structure

```
skills/repo-scout/
├── SKILL.md                  # Main file: frontmatter + five-stage workflow + output contract
├── references/
│   ├── tools.md              # Tool usage, URL rules, limitations, fallback chains
│   └── extraction.md         # Monorepo extraction strategy & token control
├── evals/
│   └── evals.json            # Behaviour evaluation cases (optional automated tests)
├── README.md                 # This file (English)
├── README.zh-CN.md           # Chinese README
└── LICENSE                   # MIT
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

## Evaluation

`evals/evals.json` holds behaviour evaluation cases (what conclusions the skill should produce). Run a "with-skill vs without-skill" comparison in any skill-eval-capable workflow to confirm the skill improves output quality. For trigger accuracy (whether the description matches correctly), test it systematically as described in the [Agent Skills description-optimization guide](https://agentskills.io/skill-creation/optimizing-descriptions).

## Known limitations

- Only **public repositories**; private repos and pages that require login are out of scope.
- Free services have anonymous rate limits and may throttle; the skill's fallback chains cover this.
- gitdiagram's diagram is LLM-generated and may be inaccurate — treat stage 4's raw-code verification as the source of truth.
- Third-party services may change or disappear; the fallbacks mitigate this, but it's worth periodically re-checking the links.

## License

[MIT](LICENSE)

## Acknowledgements

star-history, DeepWiki (open-source DeepWiki-Open), gitdiagram, gitingest, GitHub1s, uithub, Repomix and git-mcp are open-source or free online projects by their respective authors, most under the MIT license. This skill only calls their public web services and does not redistribute their source code.