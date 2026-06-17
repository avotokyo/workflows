# publish

构建并发布到 npm。

```yaml
jobs:
  publish:
    uses: avotokyo/workflows/.github/workflows/publish.yml@main
    permissions:
      contents: write
      id-token: write
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `build` | `""` | 发布前构建（空则跳过） |
| `stage` | `false` | 预发布验证 |
| `tag` | `""` | npm dist-tag |

## 示例

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

通常通过 [release.md](release.md) 一并调用。
