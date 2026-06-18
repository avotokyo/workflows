---
name: best-practices-usage
description: Permissions, path conventions, and matrix input format for avotokyo/workflows
---

# Usage Best Practices

## Path Conventions

```yaml
# Workflow — job level
uses: avotokyo/workflows/.github/workflows/<name>.yml@main

# Action — step level
uses: avotokyo/workflows/actions/<domain>/<name>@main
```

Pin `@main` or a release tag.

## Required Permissions

Reusable workflows (`workflow_call`) only receive the scopes the **calling job** grants. Permissions declared inside `avotokyo/workflows` document minimum requirements; **callers must repeat them on the job** that uses `uses: avotokyo/workflows/...`.

| Workflow / `type`                       | Caller job must pass               |
| --------------------------------------- | ---------------------------------- |
| `coverage.yml`                          | `id-token: write`                  |
| `release.yml`, `publish.yml` + `npm`    | `contents: write`, `id-token: write` |
| `release.yml`, `publish.yml` + `github` | `contents: write`, `packages: write` |
| `publish-npm.yml`                       | `contents: write`, `id-token: write` |
| `publish-github.yml`                    | `contents: read`, `packages: write`  |
| `deploy-pages.yml`                      | `contents: read`, `pages: write`, `id-token: write` |

```yaml
# npm (default)
permissions:
  contents: write
  id-token: write

# GitHub Packages
permissions:
  contents: write
  packages: write

# GitHub Pages (deploy-pages.yml)
permissions:
  contents: read
  pages: write
  id-token: write
```

## Publish Type Architecture

Publish targets are **one implementation per type**, routed by thin dispatchers:

| Layer    | Entry (with `type`)   | Implementation per type                                    |
| -------- | --------------------- | ---------------------------------------------------------- |
| Workflow | `release-publish.yml` | `release-publish-npm.yml`, `release-publish-github.yml`, … |
| Action   | —                     | `release/publish` (registry selected by `type`)            |

Callers pass `type` to `release-publish.yml`; it forwards to the matching implementation workflow.

## Adding a Publish Type

To add e.g. `type: verdaccio`:

1. **Action** — extend `actions/release/publish/action.yml` with a new `type` branch, or split setup into the per-type workflow only
2. **Workflow** — `.github/workflows/release-publish-verdaccio.yml` (setup, optional build, publish action)
3. **Dispatcher** — add one conditional job to `.github/workflows/release-publish.yml`
4. **Docs** — add `release-publish-verdaccio.md`, update [release-publish](../workflows/release-publish.md) type table

No changes needed in [composite-release](../workflows/composite-release.md) beyond documenting the new `type` value.

## Common Patterns

### CI + Coverage

```yaml
jobs:
  test:
    uses: avotokyo/workflows/.github/workflows/unit-test.yml@main

  coverage:
    needs: test
    uses: avotokyo/workflows/.github/workflows/ci-coverage.yml@main
    permissions:
      id-token: write
```

See [composite-unit-test](../workflows/composite-unit-test.md) and [ci-coverage](../workflows/ci-coverage.md).

### Deploy to GitHub Pages

```yaml
jobs:
  deploy:
    uses: avotokyo/workflows/.github/workflows/deploy-pages.yml@main
    permissions:
      contents: read
      pages: write
      id-token: write
```

See [deploy-pages](../workflows/deploy-pages.md).

## Matrix Input Format

For `ci-test.yml` / `unit-test.yml`:

```yaml
with:
  os-matrix: '"ubuntu-latest", "windows-latest"' # JSON fragment
  node-versions: "22,24,26" # comma-separated
```

## Choosing a Layer

| Need            | Use                                                                                        |
| --------------- | ------------------------------------------------------------------------------------------ |
| Quick CI        | [composite-unit-test](../workflows/composite-unit-test.md)                                 |
| Quick release   | [composite-release](../workflows/composite-release.md)                                     |
| Custom pipeline | Compose [workflows/\*](../workflows/ci-check.md) or [actions/\*](../actions/core-setup.md) |

Read one reference file at a time.
