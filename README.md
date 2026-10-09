# repo-scout

> 🌐 [中文](README.zh-CN.md) · **English**

AI skills published under the open [Agent Skills](https://agentskills.io) standard. A skill is a portable `SKILL.md` folder that works across 25+ agents — Claude Code, Cursor, Codex, Gemini CLI, OpenCode, VS Code / Copilot, Windsurf, and more — with no lock-in to any single platform.

## Skills

| Skill | Description |
| --- | --- |
| [repo-scout](skills/repo-scout/README.md) · Repository Scout | Quickly research any public GitHub repository using free online services: activity & maintenance risk, an architecture diagram, AI-friendly code material, and a structured report. |

## Install

```bash
# Auto-detects the agents you have installed (Claude Code / Cursor / Codex / OpenCode / Gemini CLI …)
npx skills add ly5566lyui/repo-scout

# Or install globally to every detected agent
npx skills add ly5566lyui/repo-scout -g --all
```

You can also copy `skills/repo-scout/` into your agent's skills directory manually — see the [skill README](skills/repo-scout/README.md).

## License

[MIT](LICENSE)