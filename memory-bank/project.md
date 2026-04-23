# project.md

## Goals

Provide a centralized, version-controlled Claude Code plugin marketplace registry for zierocode tools. The marketplace JSON is served publicly at `https://claude.zie-agent.cloud/marketplace.json`, enabling any Claude Code user to discover and install zie-plugins with a single command.

## Scope

- Maintain a single `marketplace.json` manifest that registers all zierocode plugins for Claude Code.
- Host the manifest on VPS via Caddy static file server with CORS headers.
- Auto-deploy manifest changes to VPS via GitHub Actions CI/CD pipeline.
- Register plugins: `zie-framework` (SDLC framework) and `zie-brain` (personal AI memory).
- Document install/update commands for end users.
- Provide clear plugin registration workflow (add entry → commit → push → auto-deploy).

## Non-Goals

- Plugin code does **not** live in this repo. Each plugin has its own repository under `zierocode/` on GitHub.
- No hooks, skills, or Claude Code extensions are defined here.
- No versioning of individual plugins is managed here (version bumps happen in each plugin's repo).
- No manual VPS file edits — all changes flow through this repo and CI/CD.
- No authentication or access control on the marketplace endpoint (public static file).
