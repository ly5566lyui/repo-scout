# Contributing

> 🌐 [中文](CONTRIBUTING.zh-CN.md) · **English**

Thanks for your interest in improving **repo-scout**! This repository publishes skills in the open [Agent Skills](https://agentskills.io) format, and contributions of all sizes are welcome.

## Ways to contribute

- **Report a bug** — a service stopped working, a URL rule is wrong, the output is off.
- **Suggest an idea** — a better tool, a new stage, a smarter fallback.
- **Send a PR** — fix a skill, improve the docs, or add a new skill.

For anything non-trivial, please open an issue first so we can agree on the approach.

## Repository layout

```
.
├── README.md / README.zh-CN.md          # Repo-level docs (kept in sync)
├── CONTRIBUTING.md / CONTRIBUTING.zh-CN.md
├── LICENSE
└── skills/
    └── repo-scout/
        ├── SKILL.md
        ├── README.md / README.zh-CN.md
        ├── references/
        └── evals/
```

## Adding a new skill

1. Create `skills/<skill-name>/SKILL.md`.
2. `name` in the frontmatter **must equal the directory name** (lowercase letters, numbers and hyphens only — e.g. `code-review`, not `Code_Review`).
3. Write a `description` (≤ 1024 characters) that says **both what the skill does and when to use it** — it carries the entire triggering burden, so be specific. See the [description-optimization guide](https://agentskills.io/skill-creation/optimizing-descriptions).
4. Keep the `SKILL.md` body lean and push detail into `references/` files (progressive disclosure).
5. Also allowed: `license`, `compatibility`, `metadata` frontmatter fields — stick to the standard set so the skill stays portable across agents.
6. Add a `README.md` and, ideally, a `README.zh-CN.md`.
7. Optionally add `evals/evals.json` with behaviour test cases.
8. Add the skill to the table in the repo-root `README.md` (both languages).

## Style & conventions

- **Stay tool-agnostic.** Skills must not assume a specific agent or tool name — describe capabilities, not `Claude-only` commands.
- **Explain the *why*.** Prefer clear reasoning over rigid ALL-CAPS rules; future models follow intent better than orders.
- **Bilingual by default.** When you change a README, update both `README.md` and `README.zh-CN.md` so they stay in sync.
- **No new secrets.** Skills here must run on public services without keys. Don't commit tokens.

## Testing your changes

- **Behaviour** — run the cases in `evals/evals.json` as a "with-skill vs without-skill" comparison and confirm the skill actually improves the output.
- **Triggering** — sanity-check that your `description` both fires on the right prompts and stays quiet on near-misses.
- **Links** — click through the URLs in `references/tools.md`; services come and go.

## Commit & PR guidelines

- Keep commits focused; write a clear message (imperative mood, e.g. `fix: correct gitingest sub-path URL`).
- In the PR description, say what changed and why, and note any service that behaved differently than the docs claim.
- One skill per PR where practical.

## License

By contributing, you agree that your contributions are licensed under the [MIT license](LICENSE).