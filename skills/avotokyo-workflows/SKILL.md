---
name: avotokyo-workflows
description: Use avotokyo/workflows reusable GitHub Actions for TypeScript/Vite+ projects. Use when wiring CI/CD, choosing check/test/release/coverage workflows, or referencing avotokyo/workflows actions.
metadata:
  author: avotokyo
  version: "2026.6.18"
  source: https://github.com/avotokyo/workflows
---

# avotokyo/workflows

Reusable GitHub Actions for TypeScript / Vite+ (`vp`) projects.

Repo: `avotokyo/workflows`. Pin with `@main` or a release tag.

**Important:** Workflows go in `.github/workflows/` (job-level `uses:`). Actions go in `actions/` (step-level `uses:`). Default commands: `vp check`, `vp run build`, `vp test`.

> Based on avotokyo/workflows, updated 2026-06-18.

## Documentation

| Doc | Audience | Content |
| --- | -------- | ------- |
| [README](../../README.md) | Humans | Quick install, recommended entry points, minimal CI / release examples |
| This skill | Agents | Full component inventory, inputs, permissions, patterns, extension guides |

Read **one** reference file below for the component you need. Do not load all references at once.

## Full Component Inventory

### Composite Workflows

Recommended entry points.

| Name | Workflow file | Description | Reference |
| ---- | ------------- | ----------- | --------- |
| Unit Test | `unit-test.yml` | Check + matrix test CI | [composite-unit-test](references/composite-unit-test.md) |
| Release | `release.yml` | Changelog + publish by `type` | [composite-release](references/composite-release.md) |

### Atomic Workflows

Single-job workflows. Compose manually for finer control.

| Name | Workflow file | Description | Reference |
| ---- | ------------- | ----------- | --------- |
| Check | `check.yml` | Lint/check on one OS | [workflow-check](references/workflow-check.md) |
| Test | `test.yml` | Matrix OS × Node test | [workflow-test](references/workflow-test.md) |
| Coverage | `coverage.yml` | Coverage test + Codecov | [workflow-coverage](references/workflow-coverage.md) |
| Changelog | `changelog.yml` | GitHub Release changelog | [workflow-changelog](references/workflow-changelog.md) |
| Publish | `publish.yml` | Dispatch publish by `type` | [workflow-publish](references/workflow-publish.md) |
| Publish NPM | `publish-npm.yml` | Build + npm Registry publish | [workflow-publish-npm](references/workflow-publish-npm.md) |
| Publish GitHub | `publish-github.yml` | Build + GitHub Packages publish | [workflow-publish-github](references/workflow-publish-github.md) |
| Autofix | `autofix.yml` | Auto-fix and commit | [workflow-autofix](references/workflow-autofix.md) |

### Actions

Composite steps for custom jobs.

| Name | Path | Description | Reference |
| ---- | ---- | ----------- | --------- |
| Setup | `actions/setup` | Checkout + Vite+ environment | [action-setup](references/action-setup.md) |
| Run | `actions/run` | Execute a shell command | [action-run](references/action-run.md) |
| Changelog | `actions/changelog` | Run changelogithub (step) | [action-changelog](references/action-changelog.md) |
| Publish | `actions/publish` | `vp pm publish` by `type` | [action-publish](references/action-publish.md) |
| Upload Codecov | `actions/upload-codecov` | Codecov OIDC upload | [action-upload-codecov](references/action-upload-codecov.md) |
| Autofix Commit | `actions/autofix-commit` | autofix-ci commit | [action-autofix-commit](references/action-autofix-commit.md) |

## Best Practices

| Topic | Description | Reference |
| ----- | ----------- | --------- |
| Usage | Permissions, paths, matrix format, CI + coverage, choosing a layer | [best-practices-usage](references/best-practices-usage.md) |

## Quick Reference

```yaml
# Workflow — under jobs
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

# Action — under steps
steps:
  - uses: avotokyo/workflows/actions/setup@main
```
