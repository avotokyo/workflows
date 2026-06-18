# 架构说明

avotokyo/workflows 采用三层模型组织可复用的 GitHub Actions 组件。

## 三层模型

```
消费方仓库
    │
    ├─ job  uses: .github/workflows/unit-test.yml 等   ← 组合 Workflow（推荐入口）
    │       │
    │       ├─ uses: .github/workflows/ci-*.yml        ← 原子 Workflow
    │       └─ uses: .github/workflows/release-*.yml
    │
    └─ step uses: actions/<domain>/*                ← Composite Action
            └─ 内部再 uses: actions/checkout, voidzero-dev/setup-vp 等第三方
```

| 层级             | 目录 / 命名                                  | 引用方式        | 职责                                      |
| ---------------- | -------------------------------------------- | --------------- | ----------------------------------------- |
| 组合 Workflow    | `.github/workflows/unit-test.yml` 等         | job 级 `uses:`  | 编排多个原子 workflow，提供 CI / 发布入口 |
| 原子 Workflow    | `.github/workflows/ci-*.yml`、`release-*`  | job 级 `uses:`  | 单 job 工作流，可单独使用或组合           |
| Composite Action | `actions/core/`、`release/`、`integrations/` | step 级 `uses:` | 可复用步骤，供 workflow 或自定义 job 调用 |

> **GitHub 限制：** 可复用 workflow（`workflow_call`）必须位于 `.github/workflows/` 顶层，不能放在子目录。因此所有 workflow 文件均使用 `ci-check.yml`、`release-publish.yml` 等扁平命名。

## 目录结构

```
.github/workflows/
├── unit-test.yml           # 组合入口：Check + 矩阵测试
├── release.yml             # 组合入口：Changelog + 发布
├── ci-check.yml            # CI 原子 workflow
├── ci-test.yml
├── ci-coverage.yml
├── ci-autofix.yml
├── release-changelog.yml   # 发布原子 workflow
├── release-publish.yml         # 分发器（按 type 路由）
├── release-publish-npm.yml
└── release-publish-github.yml

actions/
├── core/               # 基础步骤
│   ├── setup/          # checkout + setup-vp
│   └── run/            # 通用 shell 命令
├── release/            # 发布相关
│   ├── changelog/
│   └── publish/
└── integrations/       # 第三方集成
    ├── upload-codecov/
    └── autofix-commit/
```

## 组合模式

### CI 组合（unit-test）

`unit-test.yml` 并行调用：

- `ci-check.yml` — 单 OS lint/check
- `ci-test.yml` — 多 OS × Node 矩阵测试

### 发布组合（release）

`release.yml` 并行调用：

- `release-changelog.yml` — GitHub Release + changelog（可选）
- `release-publish.yml` — 按 `type` 分发到具体发布 workflow（可选）

### 发布分发（publish）

`release-publish.yml` 按 `inputs.type` 路由：

| type     | 目标 workflow                |
| -------- | ---------------------------- |
| `npm`    | `release-publish-npm.yml`    |
| `github` | `release-publish-github.yml` |

Action 层 `actions/release/publish` 同样按 `type` 选择 registry，与 workflow 分发模式对应。

## 引用约定

```yaml
# 消费方引用 Workflow
uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

# 消费方引用 Action
uses: avotokyo/workflows/actions/core/setup@main
```

- 仓库内部 workflow 互引用使用相对路径：`./.github/workflows/ci-check.yml`
- 跨 repo 引用需 pin `@main` 或 release tag

## 文档

| 文档                                                   | 受众  | 内容                  |
| ------------------------------------------------------ | ----- | --------------------- |
| [README.md](README.md)                                 | 人类  | 快速上手              |
| [skills/avotokyo-workflows](skills/avotokyo-workflows) | Agent | 完整组件清单与 inputs |
