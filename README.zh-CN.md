# repo-scout

> 🌐 **中文** · [English](README.md)

**用 Agent Skills 快速看懂一个陌生的 GitHub 仓库。**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-blueviolet.svg)](https://agentskills.io)
[![GitHub stars](https://img.shields.io/github/stars/ly5566lyui/repo-scout?style=social)](https://github.com/ly5566lyui/repo-scout/stargazers)

## 关于

本仓库以开放的 [Agent Skills](https://agentskills.io) 格式发布 AI 技能。技能是一个可移植的 `SKILL.md` 文件夹，可用于 Claude Code、Cursor、Codex、Gemini CLI、OpenCode、VS Code / Copilot、Windsurf、Cline、Roo Code 等 **25+ 种 agent**，不绑定任何单一平台。

主打技能 **[repo-scout（仓库速览）](skills/repo-scout/README.zh-CN.md)** 能把任意公开 GitHub 仓库变成一份结构化调研报告——活跃度与维护风险、架构图、AI 友好素材——**全程免费在线服务，无需本地安装、无需 API Key**。

## 技能列表

| 技能 | 说明 |
| --- | --- |
| [**repo-scout**](skills/repo-scout/README.zh-CN.md) · 仓库速览 | 五阶段流水线（评估 → 架构 → 提取 → 深读 → 沉淀），调研仓库并输出固定的五段式结论。[English](skills/repo-scout/README.md) |

## 特性

- 🧩 **跨 agent 通用** —— 一份 `SKILL.md`，跑在 25+ 种实现同一开放标准的 agent 上。
- 🆓 **零配置** —— 纯网页服务，不装工具、不用 API Key、无需注册。
- 🛡️ **不卡死** —— 每个阶段都配有回退链，任一步失败都有替代路径。
- 🧠 **控 token** —— 有选择性地提取素材，大 monorepo 也不爆上下文。
- 📋 **输出稳定** —— 永远是固定的五段结构，便于对比与自动化。

## 安装

```bash
# 自动识别本机已安装的 agent 并安装
npx skills add ly5566lyui/repo-scout

# 或全局安装到所有已识别的 agent
npx skills add ly5566lyui/repo-scout -g --all

# 只装到指定 agent
npx skills add ly5566lyui/repo-scout -a claude-code -a cursor
```

想手动装也行：把 `skills/repo-scout/` 放进你的 agent 技能目录即可——[技能 README](skills/repo-scout/README.zh-CN.md#安装) 里列了每个 agent 的确切路径（`.agents/skills/`、`.claude/skills/`、`.cursor/skills/`、`~/.config/opencode/skills/` 等）。

## 使用

装好之后，直接对 agent 说话即可：

- "调研一下这个仓库：https://github.com/pallets/flask"
- "这个开源项目靠谱吗？帮我评估一下活跃度和维护风险"
- "帮我快速理解这个 repo 的架构和关键代码"
- "把仓库 X 整理成适合喂给 LLM 的素材"

你会得到一份固定形状的报告：

```
1. 项目定位          2. 架构要点          3. 关键目录
4. 活跃度与风险      5. 建议后续动作
```

## 仓库结构

```
.
├── README.md            # 英文说明
├── README.zh-CN.md      # 本文件（中文）
├── CONTRIBUTING.md      # 贡献指南（英文）
├── CONTRIBUTING.zh-CN.md
├── LICENSE              # MIT
└── skills/
    └── repo-scout/
        ├── SKILL.md          # 技能本体：frontmatter + 五阶段工作流 + 输出契约
        ├── README.md         # 技能完整说明（英文）
        ├── README.zh-CN.md   # 技能完整说明（中文）
        ├── LICENSE           # 随技能自带，保持自包含
        ├── references/
        │   ├── tools.md      # 工具用法、替换规则、限制与回退链
        │   └── extraction.md # monorepo 提取策略与 token 控制
        └── evals/
            └── evals.json    # 行为评估用例
```

## 文档

| 主题 | 位置 |
| --- | --- |
| 技能做什么、怎么用 | [skills/repo-scout/README.zh-CN.md](skills/repo-scout/README.zh-CN.md) |
| 工具细节、替换规则与回退 | [skills/repo-scout/references/tools.md](skills/repo-scout/references/tools.md) |
| monorepo 提取策略 | [skills/repo-scout/references/extraction.md](skills/repo-scout/references/extraction.md) |
| Agent Skills 标准 | [agentskills.io](https://agentskills.io) |

## 贡献

欢迎提想法、报 bug、发 PR —— 见 [CONTRIBUTING.zh-CN.md](CONTRIBUTING.zh-CN.md)，里面也写了**如何在本仓库新增一个技能**。

## 许可证

基于 [MIT 许可证](LICENSE) 发布。

## 致谢

`repo-scout` 编排了这些公开网页服务：**star-history、DeepWiki、gitdiagram、gitingest、GitHub1s、uithub、Repomix、git-mcp** —— 它们均为各自作者的开源或免费项目，多数采用 MIT 协议。本仓库仅调用其公开服务，未分发其源码。