# avotokyo/workflows

面向 TypeScript / Vite+ 项目的可复用 [GitHub Actions](https://github.com/features/actions) 集合。

> [!NOTE]
> 推荐优先使用**组合 Workflow**（`unit-test`、`release`）快速接入。完整组件列表、inputs、permissions 与扩展示例见 [skills/avotokyo-workflows](./skills/avotokyo-workflows)。

## 安装

在消费方仓库的 `.github/workflows/*.yml` 中引用（将 `@main` 替换为 tag 或 commit SHA）：

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

为 Cursor 等 Agent 安装详细使用说明：

```bash
pnpx skills add avotokyo/workflows --skill=avotokyo-workflows
```

## 推荐入口

| Workflow    | 说明                                              |
| ----------- | ------------------------------------------------- |
| `unit-test` | CI：Check + 多 OS / Node 矩阵测试                 |
| `release`   | 发布：Changelog + 按 `type` 发布（npm / GitHub Packages） |

## 快速上手

### CI

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main
```

### 发布

```yaml
name: Release
on:
  push:
    tags: ["v*"]

jobs:
  release:
    uses: avotokyo/workflows/.github/workflows/release.yml@main
    with:
      publish: true
      build: vp run build
    permissions:
      contents: write
      id-token: write
```

发布到 GitHub Packages 时增加 `type: github`，并将 `permissions` 改为 `contents: write` + `packages: write`。

---

详细文档：[skills/avotokyo-workflows](./skills/avotokyo-workflows) · Forked from [sxzz/workflows](https://github.com/sxzz/workflows)
