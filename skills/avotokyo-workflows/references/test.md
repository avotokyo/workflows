# test

多 OS × 多 Node 版本矩阵运行构建与测试。

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/test.yml@main
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `os-matrix` | `'"ubuntu-latest", "windows-latest"'` | JSON 数组片段 |
| `node-versions` | `"22,24,26"` | 逗号分隔版本 |
| `build` | `vp run build` | 构建命令 |
| `test` | `vp test` | 测试命令 |

## 示例

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/test.yml@main
    with:
      os-matrix: '"ubuntu-latest"'
      node-versions: "22"
```

`os-matrix` 须为 JSON 数组片段（含内层引号）。
