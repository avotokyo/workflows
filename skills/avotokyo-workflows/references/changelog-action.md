# changelog-action

步骤级：运行 `changelogithub` 生成 Release Changelog。通常直接用 [changelog.md](changelog.md) workflow。

```yaml
steps:
  - uses: avotokyo/workflows/actions/setup@main
    with:
      fetch-all: true
      cache: false
  - uses: avotokyo/workflows/actions/changelog@main
```

无 inputs。需先 `fetch-all: true`。
