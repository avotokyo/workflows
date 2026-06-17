# avotokyo/workflows

面向 TypeScript / Vite+ 项目的可复用 [GitHub Actions](https://github.com/features/actions) 集合，包含 composite actions 与可复用 workflows，并提供配套的 [Agent Skill](https://agentskills.io/home) 供 AI 按需查阅用法。

> [!NOTE]
> 推荐优先使用**组合 Workflow**（`unit-test`、`release`）快速接入；需要更细粒度控制时再选用原子 workflow 或 action。

## 安装

### 在项目中引用 Workflow

在消费方仓库的 `.github/workflows/*.yml` 中通过 `uses` 引用（将 `@main` 替换为 tag 或 commit SHA）：

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

### 安装 Agent Skill

为 Cursor 等 Agent 安装本仓库的使用说明（按需加载，不占用多余上下文）：

```bash
pnpx skills add avotokyo/workflows --skill=avotokyo-workflows
```

全局安装：

```bash
pnpx skills add avotokyo/workflows --skill=avotokyo-workflows -g
```

Skill 路径：[skills/avotokyo-workflows](./skills/avotokyo-workflows)。详见 [skills CLI](https://github.com/vercel-labs/skills)。

## Workflows

### 组合 Workflows

> 推荐入门

开箱即用的完整流程，编排多个原子 workflow。

| Workflow  | 说明                          | 路径                                                                   |
| --------- | ----------------------------- | ---------------------------------------------------------------------- |
| unit-test | Check + 多 OS / Node 矩阵测试 | [`.github/workflows/unit-test.yml`](./.github/workflows/unit-test.yml) |
| release   | Changelog + npm 发布          | [`.github/workflows/release.yml`](./.github/workflows/release.yml)     |

### 原子 Workflows

单一职责，可按需自行组合。

| Workflow  | 说明                  | 路径                                                                   |
| --------- | --------------------- | ---------------------------------------------------------------------- |
| check     | 单 OS 代码检查        | [`.github/workflows/check.yml`](./.github/workflows/check.yml)         |
| test      | 多 OS / Node 矩阵测试 | [`.github/workflows/test.yml`](./.github/workflows/test.yml)           |
| changelog | 生成 GitHub Changelog | [`.github/workflows/changelog.yml`](./.github/workflows/changelog.yml) |
| publish   | 构建并发布 npm        | [`.github/workflows/publish.yml`](./.github/workflows/publish.yml)     |
| coverage  | 覆盖率测试 + Codecov  | [`.github/workflows/coverage.yml`](./.github/workflows/coverage.yml)   |
| autofix   | 自动修复并提交        | [`.github/workflows/autofix.yml`](./.github/workflows/autofix.yml)     |

引用格式：

```yaml
uses: avotokyo/workflows/.github/workflows/<name>.yml@main
```

## Actions

可在任意 job 的 `steps` 中引用的 composite actions。

| Action         | 说明                         | 路径                                                            |
| -------------- | ---------------------------- | --------------------------------------------------------------- |
| setup          | Checkout + 安装 Vite+ 与依赖 | [`actions/setup`](./actions/setup/action.yml)                   |
| run            | 执行 shell 命令              | [`actions/run`](./actions/run/action.yml)                       |
| changelog      | 运行 `changelogithub`        | [`actions/changelog`](./actions/changelog/action.yml)           |
| publish-npm    | `vp pm publish`              | [`actions/publish-npm`](./actions/publish-npm/action.yml)       |
| upload-codecov | 上传覆盖率到 Codecov         | [`actions/upload-codecov`](./actions/upload-codecov/action.yml) |
| autofix-commit | autofix-ci 提交变更          | [`actions/autofix-commit`](./actions/autofix-commit/action.yml) |

引用格式：

```yaml
uses: avotokyo/workflows/actions/<name>@main
```

## 快速上手

### CI

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

### CI + 覆盖率

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

  coverage:
    needs: test
    uses: avotokyo/workflows/.github/workflows/coverage.yml@main
    permissions:
      id-token: write
```

### 发布

```yaml
name: Release
on:
  push:
    tags: ["v*"]

jobs:
  release:
    uses: avotokyo/workflows/.github/workflows/release.yml@main
    with:
      publish: true
      build: vp run build
    permissions:
      contents: write
      id-token: write
```

更多示例与 inputs 说明见 [skills/avotokyo-workflows](./skills/avotokyo-workflows)。

## 目录结构

```
.
├── actions/                  # Composite Actions（step 级）
├── .github/workflows/        # 可复用 Workflows（job 级）
└── skills/avotokyo-workflows/  # Agent Skill（按需查阅）
```

| 层级           | 路径                      | 用途              |
| -------------- | ------------------------- | ----------------- |
| Actions        | `actions/*/action.yml`    | 最小可复用步骤    |
| 原子 Workflows | `.github/workflows/*.yml` | 单 job、单职责    |
| 组合 Workflows | `.github/workflows/*.yml` | 编排原子 workflow |

---

Forked from [sxzz/workflows](https://github.com/sxzz/workflows).
