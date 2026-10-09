# 贡献指南

> 🌐 **中文** · [English](CONTRIBUTING.md)

感谢你有兴趣改进 **repo-scout**！本仓库以开放的 [Agent Skills](https://agentskills.io) 格式发布技能，欢迎任何规模的贡献。

## 可以怎么贡献

- **报 bug** —— 某个服务失效、替换规则写错、输出不对。
- **提想法** —— 更好的工具、新的阶段、更聪明的回退。
- **发 PR** —— 修技能、改文档，或新增一个技能。

非小改动的话，建议先开 issue 对齐方案。

## 仓库结构

```
.
├── README.md / README.zh-CN.md          # 仓库级文档（保持同步）
├── CONTRIBUTING.md / CONTRIBUTING.zh-CN.md
├── LICENSE
└── skills/
    └── repo-scout/
        ├── SKILL.md
        ├── README.md / README.zh-CN.md
        ├── references/
        └── evals/
```

## 新增一个技能

1. 新建 `skills/<技能名>/SKILL.md`。
2. frontmatter 的 `name` **必须与目录名一致**（只能用小写字母、数字、连字符——例如 `code-review`，不能是 `Code_Review`）。
3. 写一个 `description`（≤ 1024 字符），要**同时说清"做什么"和"何时用"**——它承担全部触发职责，务必具体。参考[描述优化指南](https://agentskills.io/skill-creation/optimizing-descriptions)。
4. `SKILL.md` 正文保持精简，细节下沉到 `references/` 文件（渐进披露）。
5. 也可用 `license`、`compatibility`、`metadata` 字段——但请守住标准字段集合，保证跨 agent 可移植。
6. 补一份 `README.md`，最好再补 `README.zh-CN.md`。
7. 可选：加 `evals/evals.json` 行为测试用例。
8. 在仓库根 `README.md` 的技能表里登记（中英两版都要）。

## 风格与约定

- **保持工具无关。** 技能不能假定某个具体 agent 或工具名——描述"能力"，而不是 `Claude 专属`命令。
- **讲清"为什么"。** 比起一味用全大写硬性规则，清晰的理由更利于模型理解意图。
- **默认双语。** 改动某个 README 时，请同时更新 `README.md` 与 `README.zh-CN.md`，保持一致。
- **不引入密钥。** 本仓库的技能必须能靠公开服务、无 key 运行，不要把 token 提交进来。

## 测试你的改动

- **行为** —— 用 `evals/evals.json` 里的用例跑"有技能 vs 无技能"对比，确认技能确实提升了输出。
- **触发** —— 检查你的 `description` 该触发时会触发、近义干扰时不会误触发。
- **链接** —— 点一遍 `references/tools.md` 里的 URL，服务是会变的。

## 提交与 PR 约定

- 提交尽量聚焦；写清楚的 message（祈使句，例如 `fix: 修正 gitingest 子路径 URL`）。
- PR 描述里说明改了什么、为什么，以及有没有哪个服务的行为和文档不一致。
- 尽量一个 PR 只动一个技能。

## 许可证

参与贡献即表示你同意你的贡献按 [MIT 许可证](LICENSE) 授权。