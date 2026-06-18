# Actions

Composite actions for TypeScript / Vite+ projects. Used at step level within workflows or custom jobs.

## Core

| 路径 | 说明 |
|------|------|
| [core/setup](core/setup) | Checkout + Vite+ 环境初始化 |
| [core/run](core/run) | 执行 shell 命令 |

## Release

| 路径 | 说明 |
|------|------|
| [release/changelog](release/changelog) | 运行 changelogithub |
| [release/publish](release/publish) | `vp pm publish`，按 `type` 选择 registry |

## Integrations

| 路径 | 说明 |
|------|------|
| [integrations/upload-codecov](integrations/upload-codecov) | Codecov OIDC 上传 |
| [integrations/autofix-commit](integrations/autofix-commit) | autofix-ci 自动 commit |

## 引用方式

```yaml
steps:
  - uses: avotokyo/workflows/actions/core/setup@main
```

详见 [ARCHITECTURE.md](../ARCHITECTURE.md) 与 [MIGRATION.md](../MIGRATION.md)。
