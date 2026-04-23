# active-context.md

## Current Work

- Memory-bank initialization (this task).
- No active feature work or plugin additions in progress.

## Decisions Made

- **Marketplace is static-only:** Confirmed separation — no plugin code, no hooks, no skills in this repo. Each plugin lives in its own repository under `zierocode/`.
- **CI/CD trigger scoped to manifest changes:** `paths: [.claude-plugin/marketplace.json]` prevents unnecessary deploys on README or docs changes.
- **Tailscale + SSH pattern:** Reuses the same secure deployment pattern used by other zie repos (hermes-agent, etc.).
- **Caddy as static server:** Leverages existing Caddy container on VPS instead of adding nginx or another web server.
- **Atomic copy deploy:** `deploy.sh` copies the file rather than symlinking, ensuring clients never see a partially-written JSON.

## Next Steps

- [ ] If adding a new plugin, ensure the plugin repo has a valid `.claude-plugin/plugin.json` and a tagged release.
- [ ] Add entry to `.claude-plugin/marketplace.json` following the established schema (name, description, author, category, source.url, homepage).
- [ ] Commit and push to `main` — CI/CD auto-deploys within ~20 seconds.
- [ ] Verify live endpoint with `curl` after deploy.
- [ ] Update `README.md` plugin table if needed for visibility.
- [ ] Consider adding a `Makefile` with `make deploy` / `make verify` shortcuts for consistency with other zie repos.
