# 迁移指南

> 2026-06-19 目录重组：workflow 与 action 按功能域分子目录。所有公开路径均已变更，消费方需更新 `uses:` 引用。

## Workflow 路径对照

| 旧路径                                 | 新路径                                         |
| -------------------------------------- | ---------------------------------------------- |
| `.github/workflows/unit-test.yml`      | `.github/workflows/composite/unit-test.yml`    |
| `.github/workflows/release.yml`        | `.github/workflows/composite/release.yml`      |
| `.github/workflows/check.yml`          | `.github/workflows/ci/check.yml`               |
| `.github/workflows/test.yml`           | `.github/workflows/ci/test.yml`                |
| `.github/workflows/coverage.yml`       | `.github/workflows/ci/coverage.yml`            |
| `.github/workflows/autofix.yml`        | `.github/workflows/ci/autofix.yml`             |
| `.github/workflows/changelog.yml`      | `.github/workflows/release/changelog.yml`      |
| `.github/workflows/publish.yml`        | `.github/workflows/release/publish.yml`        |
| `.github/workflows/publish-npm.yml`    | `.github/workflows/release/publish-npm.yml`    |
| `.github/workflows/publish-github.yml` | `.github/workflows/release/publish-github.yml` |

## Action 路径对照

| 旧路径                   | 新路径                                |
| ------------------------ | ------------------------------------- |
| `actions/setup`          | `actions/core/setup`                  |
| `actions/run`            | `actions/core/run`                    |
| `actions/changelog`      | `actions/release/changelog`           |
| `actions/publish`        | `actions/release/publish`             |
| `actions/upload-codecov` | `actions/integrations/upload-codecov` |
| `actions/autofix-commit` | `actions/integrations/autofix-commit` |

## 更新示例

### CI

```yaml
# 旧
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

# 新
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/composite/unit-test.yml@main
```

### 发布

```yaml
# 旧
jobs:
  release:
    uses: avotokyo/workflows/.github/workflows/release.yml@main

# 新
jobs:
  release:
    uses: avotokyo/workflows/.github/workflows/composite/release.yml@main
```

### 自定义 Action

```yaml
# 旧
steps:
  - uses: avotokyo/workflows/actions/setup@main

# 新
steps:
  - uses: avotokyo/workflows/actions/core/setup@main
```

## 批量替换

在消费方仓库中搜索以下模式并替换：

```
.github/workflows/unit-test.yml     → .github/workflows/composite/unit-test.yml
.github/workflows/release.yml       → .github/workflows/composite/release.yml
.github/workflows/check.yml         → .github/workflows/ci/check.yml
.github/workflows/test.yml          → .github/workflows/ci/test.yml
.github/workflows/coverage.yml      → .github/workflows/ci/coverage.yml
.github/workflows/autofix.yml       → .github/workflows/ci/autofix.yml
.github/workflows/changelog.yml     → .github/workflows/release/changelog.yml
.github/workflows/publish.yml       → .github/workflows/release/publish.yml
.github/workflows/publish-npm.yml   → .github/workflows/release/publish-npm.yml
.github/workflows/publish-github.yml → .github/workflows/release/publish-github.yml

actions/setup@                     → actions/core/setup@
actions/run@                       → actions/core/run@
actions/changelog@                 → actions/release/changelog@
actions/publish@                   → actions/release/publish@
actions/upload-codecov@            → actions/integrations/upload-codecov@
actions/autofix-commit@            → actions/integrations/autofix-commit@
```

## 行为变更

本次重组**仅变更路径**，workflow inputs、defaults、permissions 逻辑均未改动。

建议 pin 到新 release tag（如 `v2026.6.19`）而非 `@main`。
