# autofix

自动修复代码风格并提交变更。

```yaml
jobs:
  autofix:
    uses: avotokyo/workflows/.github/workflows/autofix.yml@main
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `command` | `vp check --fix` | 修复命令 |
| `os` | `ubuntu-latest` | 运行环境 |

## 示例

```yaml
jobs:
  autofix:
    uses: avotokyo/workflows/.github/workflows/autofix.yml@main
    with:
      command: vp check --fix
```
