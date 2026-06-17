# check

单操作系统上运行代码检查，可选前置构建。

```yaml
jobs:
  check:
    uses: avotokyo/workflows/.github/workflows/check.yml@main
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `os` | `ubuntu-latest` | 运行环境 |
| `check` | `vp check` | 检查命令 |
| `build` | `vp run build` | 构建命令 |
| `build-for-check` | `false` | 检查前先构建 |

## 示例

```yaml
jobs:
  check:
    uses: avotokyo/workflows/.github/workflows/check.yml@main
    with:
      build-for-check: true
```
