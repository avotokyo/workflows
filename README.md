# avotokyo/workflows

面向 TypeScript / Vite+ 项目的可复用 GitHub Actions 工作流与 Action 集合。

## 组件

| 路径 | 类型 | 说明 |
| --- | --- | --- |
| [`setup-vp`](./setup-vp/action.yml) | Composite Action | 拉取代码并配置 Vite+ 开发环境 |
| [`uni-test`](./uni-test/action.yml) | Reusable Workflow | 代码检查与多平台单元测试 |
| [`coverage`](./coverage/action.yml) | Reusable Workflow | 覆盖率测试并上传至 Codecov |
| [`release`](./release/action.yml) | Reusable Workflow | 生成 Changelog、构建并发布到 npm |
| [`autofix`](./autofix/autofix.yml) | Workflow | 自动修复代码风格并提交 |

所有工作流均基于 `setup-vp` 初始化环境，CI 场景默认启用依赖缓存，发布场景拉取完整 Git 历史并关闭缓存。

## 快速开始

在仓库的 `.github/workflows/ci.yml` 中引用：

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    uses: avotokyo/workflows/uni-test/action.yml@main
```
