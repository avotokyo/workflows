# unit-test

完整 CI：并行运行检查与矩阵测试（推荐默认入口）。

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `primary-os` | `ubuntu-latest` | Check 运行环境 |
| `os-matrix` | `'"ubuntu-latest", "windows-latest"'` | Test 矩阵 OS |
| `node-versions` | `"22,24,26"` | Test 矩阵 Node |
| `check` | `vp check` | 检查命令 |
| `build` | `vp run build` | 构建命令 |
| `test` | `vp test` | 测试命令 |
| `skip-check` | `false` | 跳过检查 |
| `skip-test` | `false` | 跳过测试 |
| `build-for-check` | `false` | 检查前先构建 |

## 示例

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

仅跑测试：

```yaml
with:
  skip-check: true
```

拆分为原子 workflow 见 [check.md](check.md)、[test.md](test.md)。
