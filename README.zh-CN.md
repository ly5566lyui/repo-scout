# repo-scout

> 🌐 **中文** · [English](README.md)

以开放的 [Agent Skills](https://agentskills.io) 标准发布的 AI 技能集合。技能是通用的 `SKILL.md` 文件夹，可用于 Claude Code、Cursor、Codex、Gemini CLI、OpenCode、VS Code / Copilot、Windsurf 等 25+ 种 AI agent，不绑定任何单一平台。

## 技能列表

| 技能 | 说明 |
| --- | --- |
| [repo-scout（仓库速览）](skills/repo-scout/README.zh-CN.md) | 用免费在线服务快速调研任意公开 GitHub 仓库：评估活跃度与维护风险、生成架构图、提取 AI 友好代码素材，并输出结构化的调研结论。 |

## 安装

```bash
# 自动识别本机已安装的 agent（Claude Code / Cursor / Codex / OpenCode / Gemini CLI …）
npx skills add ly5566lyui/repo-scout

# 或全局安装到所有已识别的 agent
npx skills add ly5566lyui/repo-scout -g --all
```

也可以手动把 `skills/repo-scout/` 复制到目标 agent 的技能目录，详见[技能 README](skills/repo-scout/README.zh-CN.md)。

## 许可证

[MIT](LICENSE)