# GitHub Education Integration
_KindPath Collective — Benefits activated via GitHub Education Program_

## Active Benefits

### GitHub Team Plan (Free via Education)
- **Protected branches**: enforce PR reviews before merging to `main`
- **CODEOWNERS**: Sam auto-requested as reviewer on all PRs
- **Required status checks**: CI must pass before merge
- **Branch deletion protection**: `main` cannot be force-pushed

### GitHub Copilot Pro
- Active on VS Code + JetBrains
- Custom agents: `.github/agents/` team architecture
- Workspace instructions: `.github/copilot-instructions.md`

### GitHub Actions
- 3,000 min/month on private repos
- Active workflows: `.github/workflows/python-test.yml`

### GitHub Packages (Docker Registry)
- Registry: `ghcr.io/kindpath-collective/`
- Use to host Docker images instead of (or alongside) GCR
- See Docker workflow below

### GitHub Pages
- Enable for `kindpath-collective-website` repo
- Or use Next.js export + gh-pages branch

---

## Setup Checklist

### Branch Protection (do in GitHub UI, Sam only)
For each active repo, apply at: `github.com/KindPath-Collective/<REPO>/settings/branches`

Recommended rules for `main`:
- [x] Require a pull request before merging
- [x] Require status checks to pass (add `test` job from python-test.yml)
- [x] Do not allow bypassing the above settings
- [ ] Restrict force pushes

**Repos to protect first**: `kindai`, `kindpath-q`, `kindpath-analyser`, `.github`

### GitHub Packages — Docker (for kindai Cloud Run deploys)

Instead of `gcr.io/`, publish to `ghcr.io/`:

```yaml
# .github/workflows/docker-publish.yml
name: Build and Push Docker Image
on:
  push:
    branches: [main]
    paths:
      - 'kindai/**'
      - 'Dockerfile'

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      
      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: ghcr.io/kindpath-collective/kindai:${{ github.sha }}
```

### GitHub Pages — Public Website

Enable at: `github.com/KindPath-Collective/kindpath-collective-website/settings/pages`

```yaml
# .github/workflows/deploy-website.yml
name: Deploy Website
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '22'
      - run: npm ci && npm run build
      - uses: actions/deploy-pages@v4
```

---

## MCP Server Fix

The GitHub MCP tools were failing because they required Docker.
Fixed by:
1. Installed `github-mcp-server` binary via `go install`
2. Created `/usr/local/bin/github-mcp-wrapper` — auto-injects `gh auth token`
3. Updated `~/Library/Application Support/Code/User/mcp.json` to use wrapper

**After VS Code reload**, `mcp_github_*` tools should work without Docker.

---

## Education Account

To verify org is linked to education benefits:
- User settings: https://github.com/settings/billing
- Org settings: https://github.com/organizations/KindPath-Collective/settings/billing
- Education programme: https://education.github.com/discount_requests/application
