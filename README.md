# Claude Plugins Marketplace

Personal plugin marketplace for Claude Code, supporting skills, slash commands, and agents.

## Installation

Add this marketplace to Claude Code:

```bash
/plugin marketplace add mixomat/claude-plugins
```

Browse available plugins:
```bash
/plugin
```

Install a specific plugin:
```bash
/plugin install <plugin-name>@mixomat
```

## Available Plugins

### gitlab-code-review

Code review toolkit that uses a multi sub-agent based workflow to review local branches or GitLab merge requests.

**Install:**
```bash
/plugin install gitlab-code-review@mixomat
```

**Features:**
- `/code-review` - Run comprehensive MR reviews using specialized agents
- Supports reviewing test coverage, code quality, and project guidelines
- Aggregates findings into Critical Issues, Important Issues, and Suggestions
- Includes specialized agents for Kotlin code review and test analysis

---

### backend-development

Backend development toolkit containing supporting skills for TDD, writing use cases, and defining API specs.

**Install:**
```bash
/plugin install backend-development@mixomat
```

**Skills:**
- **brainstorm-ideas** - Refines rough ideas into fully-formed designs through collaborative questioning, alternative exploration, and incremental validation. Helps turn ideas into specs before writing code.
- **writing-use-cases** - Creates structured use case documentation with Mermaid sequence diagrams for backend systems. Documents API endpoints, service interactions, event flows, and system behaviors.

---

### git-workflow

Git workflow toolkit with commands for committing, squashing commits, and creating merge/pull requests.

**Install:**
```bash
/plugin install git-workflow@mixomat
```

**Skills:**
- **squash-commits** - Squash all commits in a feature branch into a single commit with an AI-generated summary message. Creates backup branches for safety and supports JIRA ticket patterns.
- **clean-gone-branches** - Clean up local git branches that have been deleted from the remote repository (marked as `[gone]`). Safely skips branches with worktrees.

---

## Plugin Development

See [Plugin Development Guide](https://code.claude.com/docs/en/plugins) for creating your own plugins.

## Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json      # Marketplace catalog
├── plugins/
│   ├── gitlab-code-review/   # MR review with specialized agents
│   ├── backend-development/  # TDD and use case documentation
│   └── git-workflow/         # Git commit management
└── README.md
```

## License

MIT License - see LICENSE file for details
