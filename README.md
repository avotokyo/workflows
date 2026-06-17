# avotokyo/workflows

TypeScript / Vite+ 项目的 GitHub Actions 工作流集合。

## 目录结构

```
.
├── actions/                      # 原子 Composite Actions
│   ├── setup/                    # Checkout + 安装 Vite+
│   ├── run/                      # 执行 shell 命令
│   ├── changelog/                # 生成 GitHub Changelog
│   ├── publish-npm/              # 发布到 npm
│   ├── upload-codecov/           # 上传覆盖率到 Codecov
│   └── autofix-commit/           # 提交自动修复变更
└── .github/workflows/
    ├── check.yml                 # 原子：代码检查
    ├── test.yml                  # 原子：矩阵单元测试
    ├── changelog.yml             # 原子：生成 Changelog
    ├── publish.yml               # 原子：构建并发布
    ├── coverage.yml              # 原子：覆盖率测试 + 上报
    ├── autofix.yml               # 原子：自动修复
    ├── unit-test.yml             # 组合：check + test
    └── release.yml               # 组合：changelog + publish
```

### 分层设计

| 层级               | 说明                                           |
| ------------------ | ---------------------------------------------- |
| **Actions**        | 最小可复用步骤，可在任意 job 的 `steps` 中引用 |
| **原子 Workflows** | 单一职责的可复用 workflow，对应一个 job        |
| **组合 Workflows** | 编排多个原子 workflow，提供开箱即用的完整流程  |

## 原子 Workflows

| 路径                                               | 说明                          |
| -------------------------------------------------- | ----------------------------- |
| [check.yml](./.github/workflows/check.yml)         | 单 OS 代码检查，可选前置构建  |
| [test.yml](./.github/workflows/test.yml)           | 多 OS / Node 矩阵单元测试     |
| [changelog.yml](./.github/workflows/changelog.yml) | 生成 GitHub Release Changelog |
| [publish.yml](./.github/workflows/publish.yml)     | 构建并发布到 npm              |
| [coverage.yml](./.github/workflows/coverage.yml)   | 覆盖率测试并上传 Codecov      |
| [autofix.yml](./.github/workflows/autofix.yml)     | 自动修复代码风格并提交        |

## 组合 Workflows

| 路径                                               | 组合                | 说明               |
| -------------------------------------------------- | ------------------- | ------------------ |
| [unit-test.yml](./.github/workflows/unit-test.yml) | check + test        | 完整 CI 检查与测试 |
| [release.yml](./.github/workflows/release.yml)     | changelog + publish | 完整发布流程       |

## Actions

| 路径                                                  | 说明                         |
| ----------------------------------------------------- | ---------------------------- |
| [setup](./actions/setup/action.yml)                   | Checkout + 安装 Vite+ 与依赖 |
| [run](./actions/run/action.yml)                       | 执行 shell 命令              |
| [changelog](./actions/changelog/action.yml)           | 运行 `changelogithub`        |
| [publish-npm](./actions/publish-npm/action.yml)       | 运行 `vp pm publish`         |
| [upload-codecov](./actions/upload-codecov/action.yml) | 上传覆盖率到 Codecov         |
| [autofix-commit](./actions/autofix-commit/action.yml) | 通过 autofix-ci 提交变更     |

## 使用示例

### 组合 Workflow（推荐）

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

### 原子 Workflow（按需组合）

```yaml
name: CI
on: [push, pull_request]

jobs:
  check:
    uses: avotokyo/workflows/.github/workflows/check.yml@main

  test:
    needs: check
    uses: avotokyo/workflows/.github/workflows/test.yml@main
    with:
      os-matrix: '"ubuntu-latest"'
      node-versions: "22"
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

仅发布、跳过 Changelog：

```yaml
jobs:
  publish:
    uses: avotokyo/workflows/.github/workflows/publish.yml@main
    with:
      build: vp run build
    permissions:
      contents: write
      id-token: write
```

### 单独使用 Action

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: avotokyo/workflows/actions/setup@main
        with:
          cache: true
          node-version: "22"
      - uses: avotokyo/workflows/actions/run@main
        with:
          command: vp run build
```

## Migration

| 旧路径                                         | 新路径                                                    |
| ---------------------------------------------- | --------------------------------------------------------- |
| `avotokyo/workflows/setup@main`                | `avotokyo/workflows/actions/setup@main`                   |
| `avotokyo/workflows/unit-test/action.yml@main` | `avotokyo/workflows/.github/workflows/unit-test.yml@main` |
| `avotokyo/workflows/coverage/action.yml@main`  | `avotokyo/workflows/.github/workflows/coverage.yml@main`  |
| `avotokyo/workflows/release/action.yml@main`   | `avotokyo/workflows/.github/workflows/release.yml@main`   |
| `avotokyo/workflows/autofix/action.yml@main`   | `avotokyo/workflows/.github/workflows/autofix.yml@main`   |

组合 workflow（`unit-test`、`release`）的 inputs 保持向后兼容。如需更细粒度控制，可改用对应的原子 workflow。

---

Forked from [sxzz/workflows](https://github.com/sxzz/workflows).
