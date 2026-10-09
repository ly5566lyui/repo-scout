# repo-scout · 仓库速览

> 🌐 **中文** · [English](README.md)

用一组**免费在线网页服务**，把任意公开 GitHub 仓库快速转化为可用的调研结论——评估活跃度与维护风险、生成架构图、提取 AI 友好代码素材，并按固定结构输出报告。

全程走网页服务，**无需本地安装任何工具、无需 API key**。核心技巧是把仓库链接里的 `github.com` 替换成对应服务的域名。

> 本技能遵循开放的 **Agent Skills** 标准（[agentskills.io](https://agentskills.io)），是一个通用的 `SKILL.md` 文件夹，可用于 Claude Code、Cursor、Codex、Gemini CLI、OpenCode、VS Code / Copilot、Windsurf、Cline、Roo Code 等 **25+ 种 AI agent**，不绑定任何单一平台。

## 它解决什么问题

面对一个陌生仓库时，常见的问题是：

- 这个项目活跃吗？维护得好不好？
- 整体架构怎么分？核心代码在哪？
- 仓库太大，素材怎么喂给 LLM 才不爆上下文？

repo-scout 把这些拆成**五阶段流水线**，每阶段用最合适的免费网页服务解决，并内置回退链保证流程不卡死：

| 阶段 | 做什么 | 用什么 |
| --- | --- | --- |
| 1. 快速评估 | 看星标曲线判断活跃度；查现成知识库 | star-history、DeepWiki |
| 2. 架构认知 | 生成架构图，锁定被依赖最多的核心目录 | gitdiagram |
| 3. 素材提取 | 按规模全量 / 子路径 / 过滤提取，控制 token | gitingest、repomix、uithub |
| 4. 针对性深读 | 在线核对原始代码，裁决存疑点 | GitHub1s |
| 5. 沉淀（可选） | 把仓库接成常驻 MCP 知识库 | git-mcp |

DeepWiki 命中现成 wiki 时可短路，直接跳过阶段 2、3。

## 目录结构

```
skills/repo-scout/
├── SKILL.md                  # 主文件：frontmatter + 五阶段工作流 + 输出契约
├── references/
│   ├── tools.md              # 工具用法、替换规则、限制与回退链
│   └── extraction.md         # monorepo 提取策略与 token 控制
├── evals/
│   └── evals.json            # 行为评估用例（可选的自动化测试）
├── README.md                 # 英文 README
├── README.zh-CN.md           # 本文件（中文）
└── LICENSE                   # MIT
```

## 安装

### 方式一：一键安装（推荐，自动识别你装的 agent）

用开放的 skills CLI，它会自动发现你机器上的 Cursor、Claude Code、Codex、OpenCode 等：

```bash
# 从本 GitHub 仓库安装
npx skills add ly5566lyui/repo-scout

# 或全局安装到所有已识别的 agent
npx skills add ly5566lyui/repo-scout -g --all

# 只装到指定 agent
npx skills add ly5566lyui/repo-scout -a claude-code -a cursor
```

### 方式二：手动复制

把 `skills/repo-scout` 目录（含 `SKILL.md`）放到目标 agent 的技能目录下：

| Agent | 项目级路径 | 全局路径 |
| --- | --- | --- |
| 跨客户端标准 | `.agents/skills/` | `~/.agents/skills/` |
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

> 目录名必须与 `SKILL.md` 里的 `name` 字段一致（本技能为 `repo-scout`）。跨客户端标准路径 `.agents/skills/` 会被 Codex、Cursor、Amp、OpenCode 等共同识别，适合"装一次、多端通用"。

### 方式三：不支持技能的老 agent

Windsurf、部分 Copilot 版本等若不原生支持 skills，可把 `SKILL.md` 正文粘贴到对应的规则 / 指令文件里（如 `AGENTS.md`、`.windsurfrules`、`.github/copilot-instructions.md`、`GEMINI.md`），或只粘贴其中的 frontmatter `description` + 五阶段流程。

## 使用方法

技能按 `description` 自动触发。对 AI 说类似这样的话即可：

- "调研一下这个仓库：https://github.com/xxx/yyy"
- "这个开源项目靠谱吗？帮我评估一下活跃度和维护风险"
- "帮我快速理解这个 repo 的架构和关键代码"
- "把仓库 X 整理成适合喂给 LLM 的素材"

也可以显式调用：Claude Code / Cursor 里输入 `/repo-scout`，Codex 里输入 `$repo-scout`。

执行后固定输出五个部分：

1. 项目定位
2. 架构要点
3. 关键目录
4. 活跃度与风险
5. 建议后续动作

## 支持的工具

| 工具 | 用途 | 替换规则 |
| --- | --- | --- |
| [star-history](https://star-history.com) | 星标历史曲线，判断活跃度 | 独立访问（自动跳转 www） |
| [DeepWiki](https://deepwiki.com) | 现成仓库 wiki + 问答 | 独立访问 |
| [gitdiagram](https://gitdiagram.com) | 交互式架构图（Mermaid/PNG） | github.com → gitdiagram.com |
| [gitingest](https://gitingest.com) | 结构化文本提取 + token 统计 | github.com → gitingest.com |
| [GitHub1s](https://github1s.com) | 在线 VS Code 浏览代码 | github.com → github1s.com |
| [uithub](https://uithub.com) | 更精简的 LLM 素材输出 | github → uithub |
| [Repomix](https://repomix.com) | 过滤提取 + token 统计 | 网页版粘贴 URL |
| [git-mcp](https://gitmcp.io) | 常驻 MCP 知识库 | github.com → gitmcp.io |

详细用法、限制与回退链见 [`references/tools.md`](references/tools.md)；monorepo 提取策略见 [`references/extraction.md`](references/extraction.md)。

## 评估

`evals/evals.json` 收录了行为评估用例（技能应产出什么样的结论）。可用任意支持技能评估的流程跑"有技能 vs 无技能"对比，验证技能确实提升了输出质量。触发准确性（description 是否被正确匹配）可用 [agentskills.io 的描述优化指南](https://agentskills.io/skill-creation/optimizing-descriptions) 里的方法系统性测试。

## 已知限制

- 仅支持**公开仓库**；私有仓库与需登录页面不在范围内。
- 免费服务有匿名额度限制，可能遇到限流；技能内置回退链兜底。
- gitdiagram 架构图由 LLM 生成，可能失真，关键结论以阶段 4 的原始代码核对为准。
- 第三方服务可能变更或消失；回退链已尽力兜底，但建议定期复查各链接是否可用。

## 许可证

[MIT](LICENSE)

## 致谢

star-history、DeepWiki（开源版 DeepWiki-Open）、gitdiagram、gitingest、GitHub1s、uithub、Repomix、git-mcp 均为其各自作者的开源或免费在线项目，多数采用 MIT 协议。本技能仅调用其公开网页服务，未分发其源码。