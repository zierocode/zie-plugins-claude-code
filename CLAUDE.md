# zie-plugins-claude-code

This repo is a **Claude Code plugin marketplace registry**. It contains only the marketplace manifest — no plugin code lives here.

## Structure

```
.claude-plugin/
  marketplace.json    ← single source of truth for all zie-plugins
.github/workflows/
  deploy-marketplace.yml ← auto-deploy to VPS on marketplace.json changes
CLAUDE.md             ← this file
README.md             ← public-facing docs
```

## Marketplace hosting

The marketplace JSON is served at:

```
https://claude.zie-agent.cloud/marketplace.json
```

Hosted on VPS (`zie-vps`) via Caddy static file server at `/srv/claude-marketplace/`.

## CI/CD pipeline

**Trigger:** push to `main` that changes `.claude-plugin/marketplace.json`

**Flow:** GitHub Actions → Tailscale (connect runner to tailnet) → SSH into `zie-vps` → run `/srv/claude-marketplace/deploy.sh`

**GitHub Secrets required:**
- `TS_AUTHKEY` — Tailscale auth key (ephemeral, reusable)
- `VPS_SSH_KEY` — SSH key for `zie@zie-vps`

**Deploy script** (`/srv/claude-marketplace/deploy.sh`) does:
1. `git pull` the repo to `/srv/claude-marketplace/repo/`
2. Atomic copy `marketplace.json` → serve directory

## VPS details

- **Caddy** runs as Docker container, config at `/home/zie/openclaw-docker/Caddyfile`
- **Caddy site block** for `claude.zie-agent.cloud`: static file server with CORS headers
- **Volume mount:** `/srv/claude-marketplace` → Caddy container (read-only)
- **Deploy key** (ed25519) added to VPS `authorized_keys` for CI/CD SSH access

## How to add a plugin

1. Create the plugin in its own repo (e.g. `zierocode/zie-xyz`)
2. Add an entry to the `plugins` array in `.claude-plugin/marketplace.json`:

```json
{
  "name": "zie-xyz",
  "description": "What it does",
  "author": {
    "name": "zie",
    "email": "iam@zie-agent.cloud"
  },
  "category": "development",
  "source": {
    "source": "url",
    "url": "https://github.com/zierocode/zie-xyz.git"
  },
  "homepage": "https://github.com/zierocode/zie-xyz"
}
```

3. Commit and push
4. CI/CD auto-deploys to VPS within ~20 seconds
5. Users run `claude plugin marketplace update zie-plugins` then `claude plugin install zie-xyz@zie-plugins`

## Naming convention

- Marketplace name: `zie-plugins` (set in marketplace.json)
- Plugin identifier format: `{plugin-name}@zie-plugins`
- Plugin repos live under `zierocode/` on GitHub

## Valid categories

`development` · `productivity` · `design` · `deployment` · `database` · `security` · `monitoring` · `testing` · `automation` · `learning` · `math`

## Key rules

- **Only marketplace.json lives here** — no plugin code, no hooks, no skills
- **Each plugin has its own repo** with `.claude-plugin/plugin.json`
- **Source type is always `url`** pointing to the plugin's git repo
- **Version bumps happen in each plugin's repo**, not here
- **Changes to marketplace.json auto-deploy** — no manual VPS updates needed