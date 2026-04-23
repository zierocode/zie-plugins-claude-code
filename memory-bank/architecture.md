# architecture.md

## System Design

```
GitHub repo (this repo)
  │
  │  push to main that changes marketplace.json
  │
  ▼
GitHub Actions (ubuntu-latest runner)
  │
  │  Tailscale authkey connect to tailnet
  │
  ▼
SSH into zie-vps (zie@zie-vps)
  │
  │  run /srv/claude-marketplace/deploy.sh
  │
  ▼
VPS filesystem
  ├─ /srv/claude-marketplace/repo/     ← git pull latest
  └─ /srv/claude-marketplace/marketplace.json  ← atomic copy from repo/.claude-plugin/
       │
       ▼
Caddy (Docker container)
  ├─ Config: /home/zie/openclaw-docker/Caddyfile
  ├─ Site: claude.zie-agent.cloud
  ├─ Volume: /srv/claude-marketplace → container (ro)
  └─ Serves: marketplace.json with CORS headers
       │
       ▼
Public URL: https://claude.zie-agent.cloud/marketplace.json
       │
       ▼
Claude Code clients (Mac, all projects)
  claude plugin marketplace add https://claude.zie-agent.cloud/marketplace.json
  claude plugin install zie-framework@zie-plugins --scope user
```

## Data Flow

1. **Manifest update:** Developer edits `.claude-plugin/marketplace.json` (adds a plugin entry, updates description, etc.)
2. **Commit & push to main:** Triggers `deploy-marketplace.yml` GitHub Actions workflow because `paths: [.claude-plugin/marketplace.json]`
3. **CI/CD:** GitHub runner authenticates to Tailscale, SSHs into `zie-vps`, executes `/srv/claude-marketplace/deploy.sh`
4. **Deploy script:** `git pull` the repo to `/srv/claude-marketplace/repo/`, then atomically copies `marketplace.json` to the serve directory
5. **Serve:** Caddy serves the static JSON with CORS headers; Claude Code clients fetch it on `marketplace update` or `plugin install`
6. **Plugin install:** Claude Code pulls the plugin source from the git URL specified in the plugin entry (not from this repo)

## Key Decisions

- **Static manifest only:** The repo contains zero plugin code. Separation of concerns keeps the marketplace lightweight and low-maintenance.
- **Caddy static file server:** Chosen because Caddy already runs on the VPS for other services (hermes, dashboard, etc.). Reusing the same reverse proxy stack reduces infra sprawl.
- **Atomic copy in deploy script:** Prevents serving a partially-written JSON during update.
- **Tailscale + SSH for CI/CD:** Avoids exposing SSH to the public internet. The GitHub runner joins the private tailnet to reach `zie-vps`.
- **Category enum from Anthropic schema:** Marketplace JSON uses `$schema: https://anthropic.com/claude-code/marketplace.schema.json` and valid categories (`development`, `productivity`, `design`, `deployment`, `database`, `security`, `monitoring`, `testing`, `automation`, `learning`, `math`) are enforced by Claude Code client validation.
- **Source type always `url`:** All plugins point to Git repos. No local path or tarball distribution.
