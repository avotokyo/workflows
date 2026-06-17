# release

完整发布：并行生成 Changelog 与发布 npm（推荐默认入口）。

```yaml
jobs:
  release:
    uses: avotokyo/workflows/.github/workflows/release.yml@main
    permissions:
      contents: write
      id-token: write
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `github-release` | `true` | 生成 Changelog |
| `publish` | `false` | 发布 npm |
| `stage` | `false` | 预发布验证 |
| `build` | `""` | 发布前构建 |
| `tag` | `""` | npm dist-tag |

## 示例

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

拆分为原子 workflow 见 [changelog.md](changelog.md)、[publish.md](publish.md)。
