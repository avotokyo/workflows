---
name: avotokyo-workflows
description: Use avotokyo/workflows reusable GitHub Actions in TypeScript/Vite+ projects. Use when wiring CI/CD, choosing check/test/release/coverage workflows, or referencing avotokyo/workflows actions.
---

# avotokyo/workflows

TypeScript / Vite+ (`vp`) 项目的可复用 GitHub Actions。仓库：`avotokyo/workflows`，用 `@main` 或 tag 固定版本。

## 怎么选

| 需求 | 读取 |
|------|------|
| 完整 CI | [references/unit-test.md](references/unit-test.md) |
| 仅检查 | [references/check.md](references/check.md) |
| 仅矩阵测试 | [references/test.md](references/test.md) |
| 覆盖率 | [references/coverage.md](references/coverage.md) |
| 发布 | [references/release.md](references/release.md) |
| 仅发布 npm | [references/publish.md](references/publish.md) |
| 自动修复 | [references/autofix.md](references/autofix.md) |
| 自定义 job | [references/setup.md](references/setup.md) + [references/run.md](references/run.md) |

完整索引：[references/README.md](references/README.md)

## 引用方式

```yaml
# Workflow — 写在 jobs 下
uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

# Action — 写在 steps 下
uses: avotokyo/workflows/actions/setup@main
```

## 注意

- `coverage` 需 `permissions: id-token: write`
- `release` / `publish` 需 `permissions: contents: write` + `id-token: write`
- `test` / `unit-test` 的 `os-matrix` 格式：`'"ubuntu-latest", "windows-latest"'`

按需打开 [references/](references/) 中对应文件，勿一次读取全部。
