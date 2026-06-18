# Workflows

Reusable workflows for TypeScript / Vite+ projects. All workflows use `on: workflow_call` unless noted.

## 组合入口（推荐）

| 文件                                               | 说明                              |
| -------------------------------------------------- | --------------------------------- |
| [composite/unit-test.yml](composite/unit-test.yml) | CI：Check + 多 OS / Node 矩阵测试 |
| [composite/release.yml](composite/release.yml)     | 发布：Changelog + 按 `type` 发布  |

## CI 原子 Workflow

| 文件                               | 说明                                     |
| ---------------------------------- | ---------------------------------------- |
| [ci/check.yml](ci/check.yml)       | 单 OS lint/check                         |
| [ci/test.yml](ci/test.yml)         | 多 OS × Node 矩阵测试                    |
| [ci/coverage.yml](ci/coverage.yml) | 覆盖率测试 + Codecov 上传                |
| [ci/autofix.yml](ci/autofix.yml)   | 自动修复并 commit（亦支持 push/PR 触发） |

## 发布原子 Workflow

| 文件                                                     | 说明                       |
| -------------------------------------------------------- | -------------------------- |
| [release/changelog.yml](release/changelog.yml)           | GitHub Release + changelog |
| [release/publish.yml](release/publish.yml)               | 分发器：按 `type` 路由     |
| [release/publish-npm.yml](release/publish-npm.yml)       | npm Registry 发布          |
| [release/publish-github.yml](release/publish-github.yml) | GitHub Packages 发布       |

## 引用方式

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/composite/unit-test.yml@main
```

详见 [ARCHITECTURE.md](../../ARCHITECTURE.md)。
