# 组件索引

按需读取单个文件，勿批量加载。

## 组合 Workflow（推荐入门）

| 文件 | 用途 |
|------|------|
| [unit-test.md](unit-test.md) | CI：检查 + 矩阵测试 |
| [release.md](release.md) | 发布：Changelog + npm |

## 原子 Workflow

| 文件 | 用途 |
|------|------|
| [check.md](check.md) | 单 OS 代码检查 |
| [test.md](test.md) | 多 OS / Node 矩阵测试 |
| [changelog.md](changelog.md) | 生成 GitHub Changelog |
| [publish.md](publish.md) | 构建并发布 npm |
| [coverage.md](coverage.md) | 覆盖率测试 + Codecov |
| [autofix.md](autofix.md) | 自动修复并提交 |

## Action

| 文件 | 用途 |
|------|------|
| [setup.md](setup.md) | 配置 Vite+ 环境 |
| [run.md](run.md) | 执行命令 |
| [changelog-action.md](changelog-action.md) | 生成 Changelog（步骤级） |
| [publish-npm.md](publish-npm.md) | 发布 npm（步骤级） |
| [upload-codecov.md](upload-codecov.md) | 上传 Codecov（步骤级） |
| [autofix-commit.md](autofix-commit.md) | 提交 autofix 变更（步骤级） |

## 旧路径迁移

| 旧 | 新 |
|----|-----|
| `avotokyo/workflows/setup@main` | `avotokyo/workflows/actions/setup@main` |
| `avotokyo/workflows/*/action.yml@main` | `avotokyo/workflows/.github/workflows/<name>.yml@main` |
