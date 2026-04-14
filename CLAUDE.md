# zie-plugins-claude-code

This repo is a **Claude Code plugin marketplace registry**. It contains only the marketplace manifest — no plugin code lives here.

## Structure

```
.claude-plugin/
  marketplace.json    ← single source of truth for all zie-plugins
CLAUDE.md             ← this file
README.md             ← public-facing docs
```

## How to add a plugin

1. Create the plugin in its own repo (e.g. `zierocode/zie-xyz`)
2. Add an entry to the `plugins` array in `.claude-plugin/marketplace.json`:

```json
{
  "name": "zie-xyz",
  "description": "What it does",
  "category": "development",
  "source": {
    "source": "url",
    "url": "https://github.com/zierocode/zie-xyz.git"
  },
  "homepage": "https://github.com/zierocode/zie-xyz"
}
```

3. Commit and push
4. Users run `claude plugin marketplace update zie-plugins` then `claude plugin install zie-xyz@zie-plugins`

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