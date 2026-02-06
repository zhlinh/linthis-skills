---
name: linthis.plugin
description: Manage linthis plugins for custom linters and rules
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
---

# /linthis.plugin

Manage linthis plugins for custom linters, rules, and integrations.

## Usage

```
/linthis.plugin [list|add|remove|enable|disable|info] [plugin-name]
```

## Subcommands

### list

List all installed plugins and their status.

```
/linthis.plugin list
```

### add

Add a plugin from a directory, git repo, or registry.

```
/linthis.plugin add <source>
```

Examples:
```
/linthis.plugin add ./my-plugin
/linthis.plugin add https://github.com/user/linthis-plugin-foo
/linthis.plugin add company-standards
```

### remove

Remove an installed plugin.

```
/linthis.plugin remove <plugin-name>
```

### enable

Enable a disabled plugin.

```
/linthis.plugin enable <plugin-name>
```

### disable

Disable a plugin without removing it.

```
/linthis.plugin disable <plugin-name>
```

### info

Show detailed information about a plugin.

```
/linthis.plugin info <plugin-name>
```

## Workflow

1. Check plugin directory exists (`.linthis/plugins/`)
2. Perform requested operation
3. Update plugin configuration
4. Validate plugin integrity

## Execution

```bash
PLUGIN_DIR=".linthis/plugins"

case "$SUBCOMMAND" in
    list)
        echo "Installed plugins:"
        if [ -d "$PLUGIN_DIR" ]; then
            for plugin in "$PLUGIN_DIR"/*/; do
                name=$(basename "$plugin")
                if grep -q "enabled = true" "$plugin/plugin.toml" 2>/dev/null; then
                    echo "  ✓ $name (enabled)"
                else
                    echo "  ○ $name (disabled)"
                fi
            done
        else
            echo "  No plugins installed"
        fi
        ;;
    add)
        linthis plugin add $ARGS
        echo "✓ Plugin added successfully"
        ;;
    remove)
        linthis plugin remove $ARGS
        echo "✓ Plugin removed"
        ;;
    enable)
        linthis plugin enable $ARGS
        echo "✓ Plugin enabled"
        ;;
    disable)
        linthis plugin disable $ARGS
        echo "✓ Plugin disabled"
        ;;
    info)
        linthis plugin info $ARGS
        ;;
    *)
        echo "Usage: /linthis.plugin [list|add|remove|enable|disable|info] [plugin-name]"
        ;;
esac
```

## Plugin Configuration

Plugins are configured in `.linthis/config.toml`:

```toml
[plugins]
# Plugin directory
plugin_dir = ".linthis/plugins"

# Individual plugin settings
[plugins.company-standards]
enabled = true
languages = ["cpp", "python"]

[plugins.custom-checker]
enabled = true
command = "python check.py"
languages = ["python"]
args = ["--strict"]
```

## Plugin Structure

A linthis plugin directory:

```
my-plugin/
├── plugin.toml       # Plugin metadata
├── rules/            # Custom lint rules
│   ├── cpp/
│   └── python/
├── formatters/       # Custom formatters
└── scripts/          # Helper scripts
```

### plugin.toml

```toml
[plugin]
name = "my-plugin"
version = "1.0.0"
description = "Custom lint rules for my project"
author = "Your Name"

[languages]
cpp = true
python = true

[rules]
# Custom rules configuration
no-magic-numbers = "error"
require-docstrings = "warning"
```

## Built-in Plugins

| Plugin | Description |
|--------|-------------|
| `company-standards` | Company C++ coding standards |
| `google-style` | Google style guide rules |
| `security-checks` | Security vulnerability detection |

## Creating a Plugin

1. Create plugin directory:
   ```bash
   mkdir -p .linthis/plugins/my-plugin
   ```

2. Create `plugin.toml`:
   ```toml
   [plugin]
   name = "my-plugin"
   version = "1.0.0"
   ```

3. Add custom rules or scripts

4. Enable the plugin:
   ```
   /linthis.plugin enable my-plugin
   ```

## Expected Output

### List

```
Installed plugins:

  ✓ company-standards (enabled)
      Languages: cpp, python
      Rules: 45 enabled

  ✓ security-checks (enabled)
      Languages: cpp, python, javascript
      Rules: 23 enabled

  ○ deprecated-api-checker (disabled)
      Languages: cpp
      Rules: 12 available
```

### Info

```
Plugin: company-standards
Version: 2.1.0
Description: Company C++ and Python coding standards

Languages:
  - cpp (35 rules)
  - python (10 rules)

Configuration:
  [plugins.company-standards]
  enabled = true
  strict_mode = false

Rules:
  ✓ naming-convention (error)
  ✓ header-guard-format (error)
  ✓ include-order (warning)
  ...
```

## Troubleshooting

### Plugin Not Found

```
Error: Plugin 'foo' not found
```

Check plugin directory exists:
```bash
ls .linthis/plugins/
```

### Plugin Load Error

```
Error: Failed to load plugin 'bar': Invalid plugin.toml
```

Validate plugin configuration:
```bash
cat .linthis/plugins/bar/plugin.toml
```
