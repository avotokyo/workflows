# setup

Checkout 并配置 Vite+ 环境（Node、依赖、缓存）。

```yaml
steps:
  - uses: avotokyo/workflows/actions/setup@main
    with:
      cache: true
      node-version: "22"
```

## Inputs

| Input | 默认 | 说明 |
|-------|------|------|
| `cache` | `true` | 依赖缓存 |
| `fetch-all` | `false` | 完整 Git 历史 |
| `node-version` | `lts/*` | Node 版本 |
| `auto-install` | `true` | 自动 `vp install` |
| `version` | `latest` | Vite+ 版本 |
| `working-directory` | — | monorepo 子包路径 |
| `node-version-file` | — | `.nvmrc` 等 |
| `persist-credentials` | `false` | Git 凭据 |
| `cache-dependency-path` | — | 锁文件路径 |
| `registry-url` / `scope` | — | npm 认证 |

## 示例：monorepo

```yaml
- uses: avotokyo/workflows/actions/setup@main
  with:
    working-directory: packages/foo
    node-version: "22"
```
