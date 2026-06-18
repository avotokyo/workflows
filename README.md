# avotokyo/workflows

面向 TypeScript / Vite+ 项目的可复用 [GitHub Actions](https://github.com/features/actions) 集合。

> [!NOTE]
> 推荐优先使用**组合 Workflow**（`composite/unit-test`、`composite/release`）快速接入。完整组件列表、inputs、permissions 与扩展示例见 [skills/avotokyo-workflows](./skills/avotokyo-workflows)。

## 安装

在消费方仓库的 `.github/workflows/*.yml` 中引用（将 `@main` 替换为 tag 或 commit SHA）：

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/composite/unit-test.yml@main
```

为 Cursor 等 Agent 安装详细使用说明：

```bash
pnpx skills add avotokyo/workflows --skill=avotokyo-workflows
```

## 推荐入口

| Workflow    | 路径                      | 说明                                                      |
| ----------- | ------------------------- | --------------------------------------------------------- |
| `unit-test` | `composite/unit-test.yml` | CI：Check + 多 OS / Node 矩阵测试                         |
| `release`   | `composite/release.yml`   | 发布：Changelog + 按 `type` 发布（npm / GitHub Packages） |

## 快速上手

### CI

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/composite/unit-test.yml@main
```

### 发布

```yaml
name: Release
on:
  push:
    tags: ["v*"]

jobs:
  release:
    uses: avotokyo/workflows/.github/workflows/composite/release.yml@main
    with:
      publish: true
      build: vp run build
    permissions:
      contents: write
      id-token: write
```

发布到 GitHub Packages 时增加 `type: github`，并将 `permissions` 改为 `contents: write` + `packages: write`。

---

## 文档

| 文档                                                         | 说明                              |
| ------------------------------------------------------------ | --------------------------------- |
| [ARCHITECTURE.md](./ARCHITECTURE.md)                         | 三层架构与目录约定                |
| [MIGRATION.md](./MIGRATION.md)                               | 旧路径迁移对照（2026-06-19 重组） |
| [skills/avotokyo-workflows](./skills/avotokyo-workflows)     | Agent 完整组件参考                |
| [.github/workflows/README.md](./.github/workflows/README.md) | Workflow 索引                     |
| [actions/README.md](./actions/README.md)                     | Action 索引                       |

Forked from [sxzz/workflows](https://github.com/sxzz/workflows)
