# zie-plugins-claude-code

> Claude Code plugin marketplace for [zierocode](https://github.com/zierocode) tools.

## Plugins

| Plugin | Description | Repo |
|--------|-------------|------|
| [zie-framework](https://github.com/zierocode/zie-framework) | Solo developer SDLC framework — spec-first, TDD, automated testing | [source](https://github.com/zierocode/zie-framework) |

## Install

### Add the marketplace

```bash
claude plugin marketplace add https://claude.zie-agent.cloud/marketplace.json
```

### Install a plugin

```bash
# User scope (available in all projects)
claude plugin install zie-framework@zie-plugins --scope user

# Project scope (available only in the current project)
claude plugin install zie-framework@zie-plugins --scope project
```

### Update plugins

```bash
claude plugin marketplace update zie-plugins
claude plugin update zie-framework
```

## Add a new plugin

To register a new plugin, add an entry to the `plugins` array in `.claude-plugin/marketplace.json`:

```json
{
  "name": "plugin-name",
  "description": "Short description of the plugin",
  "author": {
    "name": "zie",
    "email": "iam@zie-agent.cloud"
  },
  "category": "development",
  "source": {
    "source": "url",
    "url": "https://github.com/zierocode/plugin-name.git"
  },
  "homepage": "https://github.com/zierocode/plugin-name"
}
```

Valid categories: `development`, `productivity`, `design`, `deployment`, `database`, `security`, `monitoring`, `testing`, `automation`, `learning`, `math`

## License

MIT