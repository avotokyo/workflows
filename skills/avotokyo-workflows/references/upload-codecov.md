# upload-codecov

步骤级：上传覆盖率到 Codecov。通常直接用 [coverage.md](coverage.md) workflow。

```yaml
steps:
  - uses: avotokyo/workflows/actions/upload-codecov@main
```

无 inputs。Job 需 `id-token: write`。
