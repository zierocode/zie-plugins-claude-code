# progress.md

## Completed

- [x] Repository created with `README.md`, `CLAUDE.md`, and `ECOSYSTEM.md`.
- [x] `marketplace.json` manifest created with Anthropic schema reference.
- [x] Two plugins registered:
  - `zie-framework` — Solo developer SDLC framework for Claude Code (spec-first, TDD, automated testing, brain-integrated).
  - `zie-brain` — Personal AI memory (semantic cross-project memory for Claude Code).
- [x] GitHub Actions CI/CD workflow `deploy-marketplace.yml` implemented:
  - Triggered on push to `main` that changes `.claude-plugin/marketplace.json`.
  - Uses Tailscale for secure VPN connectivity from GitHub runner to VPS.
  - SSHs into `zie-vps` and executes `/srv/claude-marketplace/deploy.sh`.
- [x] VPS hosting configured:
  - Caddy static file server for `claude.zie-agent.cloud/marketplace.json`.
  - Volume mount: `/srv/claude-marketplace` (read-only) into Caddy container.
  - CORS headers enabled for cross-origin fetch by Claude Code clients.
- [x] Deploy script (`/srv/claude-marketplace/deploy.sh`) handles atomic `git pull` + copy of `marketplace.json`.
- [x] SSH deploy key (ed25519) added to VPS `authorized_keys` for CI/CD access.
- [x] Memory-bank created with all 5 files (`project.md`, `architecture.md`, `tech-context.md`, `active-context.md`, `progress.md`).

## In Progress

- None.

## Known Issues

- No `Makefile` yet (local dev helpers like `make deploy` / `make verify` are missing; other zie repos have this).
- If CI/CD fails, manual deploy requires SSH access to VPS — no self-service rollback UI.
- No automated validation of `marketplace.json` against the Anthropic schema in CI (only a path-based trigger exists; schema validation could catch malformed JSON before deploy).
- No health-check or uptime monitoring on `https://claude.zie-agent.cloud/marketplace.json`.
- Plugin descriptions in `marketplace.json` are duplicated in `README.md` plugin table — risk of drift when descriptions change.
