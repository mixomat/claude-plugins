# Claude Plugins Marketplace

Personal plugin marketplace for Claude Code, supporting skills, slash commands, and agents.

## Installation

Add this marketplace to Claude Code:

```bash
/plugin marketplace add mixomat/claude-plugins
```

## Available Plugins

Browse available plugins:
```bash
/plugin
```

Or install directly:
```bash
/plugin install <plugin-name>@mixomat
```

## Plugin Development

See [Plugin Development Guide](https://code.claude.com/docs/en/plugins) for creating your own plugins.

## Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json      # Marketplace catalog
├── plugins/                  # Individual plugins
└── docs/                     # Documentation
```

## License

MIT License - see LICENSE file for details
