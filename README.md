# avotokyo/workflows

TypeScript / Vite+ 项目的 GitHub Actions 工作流集合。

## 目录结构

```
.
├── actions/                  # Composite Actions
│   └── setup/
│       └── action.yml        # 配置 Vite+ 环境
└── .github/workflows/        # 可复用 Workflows
    ├── unit-test.yml         # 检查与单元测试
    ├── coverage.yml          # 覆盖率上报
    ├── release.yml           # 发布
    └── autofix.yml           # 自动修复
```

- **`actions/`** — 可在任意 job 的 `steps` 中引用的 composite action
- **`.github/workflows/`** — 通过 `jobs.<id>.uses` 调用的可复用 workflow，支持多 job、matrix 等

## Workflows

| 路径 | 说明 |
| --- | --- |
| [unit-test.yml](./.github/workflows/unit-test.yml) | Check + 多 OS / Node 矩阵测试 |
| [coverage.yml](./.github/workflows/coverage.yml) | 构建、跑覆盖率测试并上传 Codecov |
| [release.yml](./.github/workflows/release.yml) | 生成 Changelog、构建并发布到 npm |
| [autofix.yml](./.github/workflows/autofix.yml) | 自动修复代码风格并提交 |

## Actions

| 路径 | 说明 |
| --- | --- |
| [setup](./actions/setup/action.yml) | Checkout + 安装 Vite+ 与依赖 |

常用 inputs：

| Input | 默认值 | 说明 |
| --- | --- | --- |
| `cache` | `true` | 是否启用依赖缓存 |
| `fetch-all` | `false` | 是否拉取完整 Git 历史 |
| `node-version` | `lts/*` | Node.js 版本范围 |
| `auto-install` | `true` | 是否自动执行 `vp install` |

## 使用示例

### CI（单元测试）

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

### CI + 覆盖率

```yaml
name: CI
on: [push, pull_request]

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
    tags: ['v*']

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

### 自动修复

```yaml
jobs:
  autofix:
    uses: avotokyo/workflows/.github/workflows/autofix.yml@main
    with:
      command: vp check --fix
```

### 单独使用 setup action

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: avotokyo/workflows/actions/setup@main
        with:
          cache: true
          node-version: "22"
      - run: vp run build
```

## Migration

目录重组后，消费方需更新 `uses:` 路径：

| 旧路径 | 新路径 |
| --- | --- |
| `avotokyo/workflows/setup@main` | `avotokyo/workflows/actions/setup@main` |
| `avotokyo/workflows/unit-test/action.yml@main` | `avotokyo/workflows/.github/workflows/unit-test.yml@main` |
| `avotokyo/workflows/coverage/action.yml@main` | `avotokyo/workflows/.github/workflows/coverage.yml@main` |
| `avotokyo/workflows/release/action.yml@main` | `avotokyo/workflows/.github/workflows/release.yml@main` |
| `avotokyo/workflows/autofix/action.yml@main` | `avotokyo/workflows/.github/workflows/autofix.yml@main` |

---

Forked from [sxzz/workflows](https://github.com/sxzz/workflows).
