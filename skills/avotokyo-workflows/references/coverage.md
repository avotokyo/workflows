# coverage

运行覆盖率测试并上传 Codecov。

```yaml
jobs:
  coverage:
    uses: avotokyo/workflows/.github/workflows/coverage.yml@main
    permissions:
      id-token: write
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `test` | `vp test run --coverage` | 覆盖率命令 |
| `build` | `vp run build` | 测试前构建 |

## 示例

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

  coverage:
    needs: test
    uses: avotokyo/workflows/.github/workflows/coverage.yml@main
    permissions:
      id-token: write
```
