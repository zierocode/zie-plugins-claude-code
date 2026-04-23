# zie-plugins-claude-code in the zie Ecosystem

## What This Is

Claude Code plugin marketplace registry. Hosts a single `marketplace.json` served at `https://claude.zie-agent.cloud/marketplace.json`. Currently registers two plugins: `zie-framework` (SDLC framework) and `zie-brain` (cross-project memory).

## Role in the Ecosystem

- **Upstream:** zie-framework repo, zie-brain repo
- **Downstream:** Claude Code clients on all Macs
- **Data flow:** Marketplace JSON pulled by `claude plugin marketplace add`. Individual plugins pulled from their source repos.

## Key Entry Points

| Path | Purpose |
|------|---------|
| `.claude-plugin/marketplace.json` | Plugin registry (served publicly) |
| `CLAUDE.md` | CI/CD + VPS deploy details |
| `.github/workflows/deploy-marketplace.yml` | Auto-deploy on manifest change |

## Quick Commands

```bash
make deploy            # CI/CD: git push → GitHub Actions → SSH VPS → deploy.sh
```

## Ecosystem Links

- [Overview → ../hermes-agent/docs/ecosystem/README.md]
- [Related: zie-brain → ../zie-brain/ECOSYSTEM.md]
