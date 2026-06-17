# autofix-commit

步骤级：提交 autofix 产生的变更。通常直接用 [autofix.md](autofix.md) workflow。

```yaml
steps:
  - uses: avotokyo/workflows/actions/run@main
    with:
      command: vp check --fix
  - uses: avotokyo/workflows/actions/autofix-commit@main
```

无 inputs。
