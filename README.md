# linthis-skills

Claude Code skills for [linthis](https://github.com/zhlinh/linthis) - a fast, cross-platform multi-language linter and formatter.

## Features

- **Multi-language support**: C++, Python, JavaScript, TypeScript, Go, Rust, Java, Kotlin, Swift
- **AI-powered fixing**: Automated code fixes using Claude, CodeBuddy, or local LLMs
- **Git integration**: Pre-commit hooks for continuous quality
- **Rich reports**: Human-readable, HTML, Markdown, and JSON formats

## Installation

### Via Marketplace (Recommended)

```bash
# In Claude Code, add the marketplace source first
/plugin marketplace add zhlinh/linthis-skills

# Then install the plugin
/plugin install linthis-skills@zhlinh
```

Restart Claude Code to load the plugin.

### Manual Installation

```bash
# Clone the repo
git clone https://github.com/zhlinh/linthis-skills

# Install from local path
claude plugin install ./linthis-skills
```

Restart Claude Code to load the plugin.

## Updating

Skills update automatically when you update the plugin:

```bash
/plugin update linthis-skills
```

## Skills

| Skill | Description |
|-------|-------------|
| `linthis-expert` | Core linting expertise - configuration, commands, languages |
| `ai-fixer` | AI-powered code fixing with backup/restore |

## Slash Commands

| Command | Description |
|---------|-------------|
| `/linthis.check` | Run lint check on the codebase |
| `/linthis.format` | Format code using language-specific formatters |
| `/linthis.fix` | Auto-fix issues using built-in fixers |
| `/linthis.fix-ai` | Fix issues with AI assistance |
| `/linthis.report` | View and generate lint reports |
| `/linthis.hook` | Manage Git pre-commit hooks |
| `/linthis.config` | View and manage configuration |
| `/linthis.plugin` | Manage plugins for custom linters and rules |

## Quick Start

```bash
# Check for issues
/linthis.check

# Format code
/linthis.format

# Auto-fix simple issues
/linthis.fix

# Fix complex issues with AI
/linthis.fix-ai --provider claude-cli

# View detailed report
/linthis.report show

# Set up pre-commit hook
/linthis.hook install
```

## Configuration

Create `.linthis/config.toml`:

```toml
[general]
exclude = ["vendor/**", "node_modules/**"]

[languages.cpp]
enabled = true
standard = "c++17"

[languages.python]
enabled = true
linter = "ruff"
formatter = "ruff"

[hooks]
pre_commit = true
```

## AI Providers

| Provider | Command | Notes |
|----------|---------|-------|
| Claude CLI | `--provider claude-cli` | Recommended |
| Claude API | `--provider claude` | Requires API key |
| CodeBuddy | `--provider codebuddy-cli` | Tencent CodeBuddy |
| Local LLM | `--provider local` | Self-hosted |

## Requirements

- [linthis](https://github.com/zhlinh/linthis) installed
- Claude Code CLI
- Language-specific linters (cpplint, ruff, eslint, etc.)

## License

MIT
