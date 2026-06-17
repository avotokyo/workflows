# avotokyo/workflows

TypeScript / Vite+ 项目的 GitHub Actions 工作流集合。

| 路径 | 说明 |
| --- | --- |
| [`setup-vp`](./setup-vp/action.yml) | 配置 Vite+ 环境 |
| [`uni-test`](./uni-test/action.yml) | 检查与单元测试 |
| [`coverage`](./coverage/action.yml) | 覆盖率上报 |
| [`release`](./release/action.yml) | 发布 |
| [`autofix`](./autofix/autofix.yml) | 自动修复 |

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    uses: avotokyo/workflows/uni-test/action.yml@main
```

---

Inspired by [sxzz/workflows](https://github.com/sxzz/workflows).
