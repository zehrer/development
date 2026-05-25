# GitHub Pages Deployment

Web app repositories publish to GitHub Pages via a GitHub Actions workflow that triggers on every push to `main`.

## How It Works

1. Push to `main` triggers the workflow
2. The workflow builds the app (`npm run build`) and outputs to `dist/`
3. `JamesIves/github-pages-deploy-action` pushes `dist/` to the `gh-pages` branch
4. GitHub serves the `gh-pages` branch as the public site

## Repositories Using This Pattern

| Repository | Live URL |
|------------|----------|
| [web3D](https://github.com/zehrer/web3D) | https://zehrer.github.io/web3D |
| [webReasoner](https://github.com/zehrer/webReasoner) | https://zehrer.github.io/webReasoner |
| [webYearPlan](https://github.com/zehrer/webYearPlan) | https://zehrer.github.io/webYearPlan |

## Standard Workflow

Create `.github/workflows/deploy.yml` in the repository:

```yaml
name: Deploy to gh-pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: write

concurrency:
  group: gh-pages
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: lts/*
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Disable Jekyll
        run: touch dist/.nojekyll

      - name: Deploy to gh-pages
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          branch: gh-pages
          folder: dist
          clean: true
```

## Using Environment Variables at Build Time

If the build needs secrets or repository variables (e.g. API keys, client IDs), pass them via `env` in the Build step:

```yaml
      - name: Build
        env:
          VITE_GOOGLE_CLIENT_ID: ${{ vars.VITE_GOOGLE_CLIENT_ID }}
        run: npm run build
```

Set the variable under **Settings → Secrets and variables → Actions → Variables** in the repository.

## GitHub Pages Settings

In the repository, go to **Settings → Pages** and set:
- **Source**: Deploy from a branch
- **Branch**: `gh-pages` / `/ (root)`

## Notes

- `touch dist/.nojekyll` prevents GitHub from running Jekyll on the output, which can strip files starting with `_` (common in bundler output).
- The `concurrency` block cancels any in-progress deployment if a newer push arrives, preventing race conditions on the `gh-pages` branch.
