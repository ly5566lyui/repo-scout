# 工具用法 · 替换规则 · 限制 · 回退链

本文件收纳全部在线工具的细节。仅在主流程需要或遇到失败时读取。

## 工具清单

| 工具 | 入口 | 替换规则 | 用途 | 类型 |
| --- | --- | --- | --- | --- |
| star-history | `star-history.com/#<owner>/<repo>` | 独立访问（自动跳转 www） | 星标历史曲线，判断活跃度 | 独立网站 |
| DeepWiki | `deepwiki.com/<owner>/<repo>` | 独立访问 | 现成仓库 wiki + 问答 | 独立网站 |
| gitdiagram | `gitdiagram.com/<owner>/<repo>` | github.com → gitdiagram.com（hub→diagram） | 生成交互式架构图（Mermaid/PNG） | 改域名 |
| gitingest | `gitingest.com/<owner>/<repo>` | github.com → gitingest.com（hub→ingest） | 结构化文本提取 + token 统计 | 改域名 |
| GitHub1s | `github1s.com/<owner>/<repo>` | github.com → github1s.com | 在线 VS Code 浏览代码 | 改域名 |
| uithub | `uithub.com/<owner>/<repo>` | github → uithub（首字母 g→u） | 更精简的 LLM 素材输出 | 改域名 |
| Repomix 网页版 | `repomix.com` | 粘贴 URL 使用 | 过滤提取 + token 统计 | 独立网站 |
| git-mcp | `gitmcp.io/<owner>/<repo>` | github.com → gitmcp.io | 把仓库接成常驻 MCP 知识库 | 独立网站 |

> 替换规则不统一：uithub 是换首字母、gitdiagram/gitingest 是换 hub、GitHub1s 是整体换域名。执行时逐工具核对，不要猜。

## 关键限制

- **gitdiagram**：架构图由 LLM 生成，可能有失真，关键结论以原始代码为准；匿名访问有 IP 次数限制，额度耗尽需换服务或跳过。
- **DeepWiki**：热门仓库有现成 wiki，冷门仓库可能尚未生成。访问可能碰到 429 或 Vercel 安全挑战页，此时视同"没有 wiki"继续流程，不卡住。若环境提供 DeepWiki MCP 工具（如 `mcp-deepwiki.deepwiki_fetch`），**优先用 MCP**——更可靠，不受浏览器挑战页影响。
- **gitingest**：支持子路径语法 `<owner>/<repo>/tree/main/<子目录>`，可大幅缩减 token。
- **改域名四件套**（gitdiagram / gitingest / GitHub1s / uithub）：仅支持公开仓库。
- **star-history**：301 跳转到 `www.star-history.com`，浏览器自动跟随，无需处理。
- 所有服务均针对公开仓库；私有仓库无法访问。

## 无浏览器环境的数据回退

改域名类服务（gitdiagram / gitingest / uithub / GitHub1s）都是 **JS 客户端渲染**：只有静态网页抓取、没有能执行 JS 的浏览器时，可能取不到内容。此时改用 **GitHub 官方 REST API** 获取等价元数据（无需登录，匿名有配额）：

- 仓库概览：`api.github.com/repos/<owner>/<repo>`
- 近期提交：`api.github.com/repos/<owner>/<repo>/commits`
- 发布历史：`api.github.com/repos/<owner>/<repo>/releases`
- 贡献者：`api.github.com/repos/<owner>/<repo>/contributors`
- 目录 / 文件：`api.github.com/repos/<owner>/<repo>/contents/<path>`
- 星标数：仓库概览里的 `stargazers_count`

这样即使环境里没有能渲染 JS 的浏览器，活跃度与目录结构判断依然可做；架构图（阶段 2）可退化为"按 README + 顶层目录人工归纳"。这条回退对不同 agent 尤其重要——它们的网页访问能力差异很大。

## 阶段 2 回退链（架构认知）

`gitdiagram` 限流或失败时，依次尝试：

1. `gitdiagram`（首选，交互式架构图）
2. `DeepWiki` 的架构章节
3. `github1s` 手动浏览顶层目录结构，人工归纳模块划分
4. 全部不可用 → 跳过本阶段，阶段 3 改用全量或按 README 目录提取

## 阶段 3 回退链（素材提取）

1. `gitingest` 域名替换（首选，含 token 统计）
2. `repomix.com` 网页版（可用 include 过滤，本地无 Node 环境时的最佳替代）
3. `uithub`（输出更精简，适合只需概览时）
4. 全部失败 → 报告限制并询问用户是否提供本地文件

## 来源致谢

star-history、DeepWiki（开源版 DeepWiki-Open）、gitdiagram、gitingest、
GitHub1s、uithub、Repomix、git-mcp 均为其各自作者的开源或免费在线项目，
多数采用 MIT 协议。此处仅调用其公开网页服务，未分发其源码。
