# repo-scout · 仓库速览

> 🌐 **中文** · [English](README.md)

**几分钟看懂一个陌生的公开 GitHub 仓库——活跃度、架构、AI 友好素材，全程只用免费在线服务。**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-blueviolet.svg)](https://agentskills.io)

> 本技能遵循开放的 **Agent Skills** 标准（[agentskills.io](https://agentskills.io)），是一个可移植的 `SKILL.md` 文件夹，可用于 Claude Code、Cursor、Codex、Gemini CLI、OpenCode、VS Code / Copilot、Windsurf、Cline、Roo Code 等 **25+ 种 AI agent**，不绑定任何单一平台。

## 它解决什么问题

面对一个陌生仓库时，常见的问题是：

- 这个项目活跃吗？维护得好不好？
- 整体架构怎么分？核心代码在哪？
- 仓库太大，素材怎么喂给 LLM 才不爆上下文？

repo-scout 把这些从"临时抓瞎"变成一套可复用的流程。全程走网页服务，**无需本地安装、无需 API Key**。核心技巧是把仓库链接里的 `github.com` 替换成对应服务的域名。

## 为什么选 repo-scout

- **跨 agent 通用** —— 一份 `SKILL.md`，跑在 25+ 种实现同一开放标准的 agent 上，装一次到处能用。
- **零配置** —— 不用装 CLI、不用管 token、不用注册。
- **天生抗故障** —— 每个阶段都有回退链，另有"无浏览器"时走 GitHub REST API 的数据通道，遇到限流或抽风也能跑完。
- **控 token** —— 面对 monorepo 只提核心目录并报告 token 占用，不爆上下文。
- **输出稳定** —— 永远是固定的五段结构，便于对比与自动化。

## 五阶段流水线

| 阶段 | 做什么 | 用什么 |
| --- | --- | --- |
| 1. 快速评估 | 看星标曲线判断活跃度；查现成知识库 | star-history、DeepWiki |
| 2. 架构认知 | 生成架构图，锁定被依赖最多的核心目录 | gitdiagram |
| 3. 素材提取 | 按规模全量 / 子路径 / 过滤提取，控制 token | gitingest、repomix、uithub |
| 4. 针对性深读 | 在线核对原始代码，裁决存疑点 | GitHub1s |
| 5. 沉淀（可选） | 把仓库接成常驻 MCP 知识库 | git-mcp |

DeepWiki 命中现成 wiki 时可短路，直接跳过阶段 2、3。

## 实战示例

提问：*"调研一下 https://github.com/pallets/flask，它活跃吗？架构怎么分？核心代码在哪？"*

技能会产出类似这样的报告：

```markdown
## 1. 项目定位
Flask —— Pallets 出品的 Python 轻量 WSGI Web 框架。依赖 werkzeug、jinja2、click。

## 2. 架构要点
分三层：`src/flask/sansio/`（无 IO 核心，被 Quart 复用）→ `src/flask/*.py`
（WSGI 运行时）→ `src/flask/json/`。`Scaffold → App → Flask` 的继承链承载路由、
配置与 WSGI 调用路径。

## 3. 关键目录
- src/flask/app.py        —— Flask 主类、请求分发、WSGI __call__
- src/flask/sansio/       —— 共享的无 IO 核心
- src/flask/ctx.py        —— 应用/请求上下文

## 4. 活跃度与风险
最近提交在 24 小时内；最新发版约 7 个月前；未关闭 issue 仅 4 个。
活跃但节奏偏慢；bus factor 集中在单一维护者。

## 5. 建议后续动作
生产环境钉到最新 tag（3.1.3）；先读 docs/design.rst，再按顺序读上述四个关键文件。
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

> 各工具的替换规则并不统一（uithub 换首字母、gitdiagram/gitingest 换 `hub`、GitHub1s 整体换域名），技能会逐个核对，而不是套用同一条规则。

## 评估

`evals/evals.json` 收录了行为评估用例（技能应产出什么样的结论）。可用任意支持技能评估的流程跑"有技能 vs 无技能"对比，验证技能确实提升了输出质量。触发准确性（description 是否被正确匹配）可用 [agentskills.io 的描述优化指南](https://agentskills.io/skill-creation/optimizing-descriptions) 里的方法系统性测试。

## 常见问题与排错

**某个服务取不到内容 / 像是挂了。**
有时会这样——这些是免费服务，有匿名额度限制，且部分靠客户端 JS 渲染。技能的回退链会顶上；实在都不行时，会退回 GitHub REST API（`api.github.com`），公开仓库无需鉴权。

**一定要有浏览器吗？**
不一定。真正的浏览器对 JS 渲染类服务效果最好，但只有静态抓取能力的 agent 也能靠 API 回退跑通（阶段 2、3 会优雅降级）。

**能调研私有仓库吗？**
不能——仅限公开仓库，私有仓库与需登录页面明确不在范围内。

**架构图看着不准。**
gitdiagram 的图由 LLM 生成，可能失真。阶段 4（用 GitHub1s 读原始代码）才是事实来源，把图当作线索即可。

## 路线图

- [ ] 可选 GitHub token 模式，提高限额并支持私有仓库。
- [ ] 提供 `--json` 输出契约，方便机器消费。
- [ ] 接入更多工具与回退数据源。

## 已知限制

- 仅支持**公开仓库**；私有仓库与需登录页面不在范围内。
- 免费服务有匿名额度限制，可能遇到限流；技能内置回退链兜底。
- gitdiagram 架构图由 LLM 生成，可能失真，关键结论以阶段 4 的原始代码核对为准。
- 第三方服务可能变更或消失；回退链已尽力兜底，但建议定期复查各链接是否可用。

## 贡献

欢迎贡献——见仓库的 [CONTRIBUTING.zh-CN.md](https://github.com/ly5566lyui/repo-scout/blob/main/CONTRIBUTING.zh-CN.md)。

## 许可证

[MIT](LICENSE)

## 致谢

star-history、DeepWiki（开源版 DeepWiki-Open）、gitdiagram、gitingest、GitHub1s、uithub、Repomix、git-mcp 均为其各自作者的开源或免费在线项目，多数采用 MIT 协议。本技能仅调用其公开网页服务，未分发其源码。