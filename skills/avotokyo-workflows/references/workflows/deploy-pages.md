---
name: workflow-deploy-pages
description: Build and deploy static site to GitHub Pages
---

# Deploy Pages Workflow

Atomic workflow for GitHub Pages. Builds with Vite+ (`vp`), uploads a Pages artifact, and deploys via the official Pages actions.

```yaml
jobs:
  deploy:
    uses: avotokyo/workflows/.github/workflows/deploy-pages.yml@main
```

Permissions are defined inside the reusable workflow (`contents: read`, `pages: write`, `id-token: write`). Callers usually do not need to pass `permissions` again.

**Prerequisite:** Repository Settings → Pages → Source → **GitHub Actions**.

## Inputs

| Input               | Default          | Description                                      |
| ------------------- | ---------------- | ------------------------------------------------ |
| `build`             | `vp run build`   | Build command                                    |
| `artifact-path`     | `dist`           | Static output directory for `upload-pages-artifact` |
| `fetch-all`         | `false`          | Full Git history for setup (e.g. Rspress `lastUpdated`) |
| `working-directory` | `""`             | Working directory for setup                      |
| `environment`       | `github-pages`   | GitHub Pages deployment environment name         |

## Example

### Vite+ site (default)

```yaml
name: Deploy
on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  deploy:
    uses: avotokyo/workflows/.github/workflows/deploy-pages.yml@main
```

### Rspress site

```yaml
jobs:
  deploy:
    uses: avotokyo/workflows/.github/workflows/deploy-pages.yml@main
    with:
      artifact-path: doc_build
      fetch-all: true
```
