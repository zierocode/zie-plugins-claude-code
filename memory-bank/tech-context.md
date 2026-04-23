# tech-context.md

## Tech Stack

| Layer | Tech |
|-------|------|
| Version Control | Git + GitHub |
| CI/CD | GitHub Actions (`ubuntu-latest` runner) |
| VPN for CI/CD | Tailscale (`tailscale/github-action@v3`) |
| Deployment Target | VPS (Ubuntu 24.04 LTS, `zie-vps`) |
| Web Server | Caddy (Docker container, existing infra) |
| Static Hosting | `/srv/claude-marketplace/marketplace.json` served by Caddy |
| Schema | Anthropic Claude Code marketplace schema (`https://anthropic.com/claude-code/marketplace.schema.json`) |

## Dependencies

- **GitHub Secrets:**
  - `TS_AUTHKEY` — Tailscale ephemeral reusable auth key
  - `VPS_SSH_KEY` — SSH private key (ed25519) for `zie@zie-vps`, added to VPS `authorized_keys`
- **VPS filesystem paths:**
  - `/srv/claude-marketplace/repo/` — git working copy
  - `/srv/claude-marketplace/deploy.sh` — deploy script (pulled as part of repo, but CI invokes it explicitly)
  - `/srv/claude-marketplace/marketplace.json` — live served file
- **Caddy config:** Site block for `claude.zie-agent.cloud` in `/home/zie/openclaw-docker/Caddyfile`

## Build / Deploy Commands

```bash
# Local development: validate JSON before pushing
python3 -m json.tool .claude-plugin/marketplace.json > /dev/null

# Deploy changes (via CI/CD — no manual deploy needed for manifest changes)
git add .claude-plugin/marketplace.json
git commit -m "chore(marketplace): add zie-xyz plugin"
git push origin main

# Manual deploy fallback (if CI/CD fails)
ssh zie@zie-vps "bash /srv/claude-marketplace/deploy.sh"

# Verify live endpoint
curl -s https://claude.zie-agent.cloud/marketplace.json | python3 -m json.tool

# Check recent CI runs
gh run list --limit 5
```

## Key Files

| File | Purpose |
|------|---------|
| `.claude-plugin/marketplace.json` | Plugin registry manifest (single source of truth) |
| `.github/workflows/deploy-marketplace.yml` | GitHub Actions workflow: Tailscale connect + SSH deploy |
| `CLAUDE.md` | Internal architecture + CI/CD docs |
| `README.md` | Public-facing user docs |
| `ECOSYSTEM.md` | Ecosystem integration notes |

## Naming Conventions

- Marketplace name: `zie-plugins`
- Plugin identifier format: `{plugin-name}@zie-plugins`
- Plugin repos: `https://github.com/zierocode/{plugin-name}.git`
