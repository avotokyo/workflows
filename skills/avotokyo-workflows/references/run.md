# run

执行一条 shell 命令。

```yaml
steps:
  - uses: avotokyo/workflows/actions/run@main
    with:
      command: vp run build
```

## Inputs

| Input | 必填 | 说明 |
|-------|------|------|
| `command` | 是 | shell 命令 |

配合 [setup.md](setup.md) 在自定义 job 中使用。
