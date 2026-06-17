# publish-npm

步骤级：发布到 npm。通常直接用 [publish.md](publish.md) workflow。

```yaml
steps:
  - uses: avotokyo/workflows/actions/publish-npm@main
    with:
      stage: false
      tag: ""
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `stage` | `false` | `true` → 预发布验证 |
| `tag` | `""` | npm dist-tag |

Job 需 `id-token: write`。
