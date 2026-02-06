# Linthis Command Reference

## Basic Commands

### Check Mode
```bash
# Basic lint check
linthis -c

# Check with auto-fix
linthis -c -f

# Check with immediate output (don't wait for all files)
linthis -c -i

# Check specific files or directories
linthis -c src/main.cpp tests/

# Verbose output
linthis -c -v

# Quiet mode (only errors)
linthis -c -q
```

### Fix Mode
```bash
# Fix issues from last check
linthis fix

# Fix with AI assistance
linthis fix --ai --provider claude-cli

# Fix with AI and auto-accept all suggestions
linthis fix --ai --provider claude-cli --accept-all

# Undo last fix (restore from backup)
linthis fix --undo

# List available backups
linthis fix --list-backups
```

### Report Mode
```bash
# Show last result in readable format
linthis report show

# Show with limited issues
linthis report show -n 10

# Show errors only
linthis report show --errors-only

# Generate HTML report
linthis report html

# Generate Markdown report
linthis report markdown

# Open report in browser
linthis report html --open
```

### Hook Mode
```bash
# Install git hooks
linthis hook install

# Uninstall git hooks
linthis hook uninstall

# Run in hook mode (for pre-commit)
linthis --hook-mode=pre-commit -c
```

### Configuration
```bash
# Show current configuration
linthis config show

# Initialize configuration
linthis config init

# Set a configuration value
linthis config set languages.cpp.enabled true
```

## Common Options

| Option | Description |
|--------|-------------|
| `-c, --check` | Run lint check |
| `-f, --fix` | Auto-fix issues |
| `-i, --immediate` | Show output immediately |
| `-v, --verbose` | Verbose output |
| `-q, --quiet` | Quiet mode |
| `--config <path>` | Custom config file |
| `--no-cache` | Disable caching |

## AI Providers

```bash
# Claude CLI (recommended)
linthis fix --ai --provider claude-cli

# Claude API
linthis fix --ai --provider claude

# CodeBuddy CLI
linthis fix --ai --provider codebuddy-cli

# Local LLM
linthis fix --ai --provider local
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success, no issues |
| 1 | Lint errors found |
| 2 | Format issues found |
| 3 | Warnings only |
