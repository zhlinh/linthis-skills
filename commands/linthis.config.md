---
name: linthis.config
description: View and manage linthis configuration
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
---

# /linthis.config

View, initialize, and modify linthis configuration.

## Usage

```
/linthis.config [show|init|set]
```

## Subcommands

### show

Display current configuration.

```
/linthis.config show
```

### init

Initialize configuration for the project.

```
/linthis.config init
```

Creates `.linthis/config.toml` with sensible defaults based on detected languages.

### set

Set a configuration value.

```
/linthis.config set <key> <value>
```

Examples:
```
/linthis.config set languages.cpp.enabled true
/linthis.config set languages.python.formatter ruff
/linthis.config set hooks.pre_commit true
```

## Workflow

1. Check for existing `.linthis/config.toml`
2. Perform requested operation
3. Validate configuration
4. Report changes

## Execution

```bash
case "$SUBCOMMAND" in
    show)
        linthis config show
        ;;
    init)
        if [ -f .linthis/config.toml ]; then
            echo "Configuration already exists at .linthis/config.toml"
            echo "Use /linthis.config show to view it"
        else
            linthis config init
            echo "✓ Created .linthis/config.toml"
            echo "  Edit to customize language settings"
        fi
        ;;
    set)
        linthis config set $ARGS
        echo "✓ Configuration updated"
        ;;
    *)
        echo "Usage: /linthis.config [show|init|set <key> <value>]"
        ;;
esac
```

## Configuration File

`.linthis/config.toml`:

```toml
[general]
verbose = false
exclude = ["vendor/**", "node_modules/**"]

[languages.cpp]
enabled = true
standard = "c++17"
linter = "cpplint"

[languages.python]
enabled = true
linter = "ruff"
formatter = "ruff"

[hooks]
pre_commit = true
exit_on_error = true
output_width = 0  # auto-detect

[reports]
default_format = "json"
include_trends = true
```

## Language Detection

When running `init`, linthis detects:
- `Cargo.toml` → Rust
- `package.json` → JavaScript/TypeScript
- `pyproject.toml` / `setup.py` → Python
- `go.mod` → Go
- `CMakeLists.txt` / `*.cpp` → C++
- `build.gradle` / `*.java` → Java/Kotlin
- `Package.swift` → Swift

## Environment Variables

| Variable | Description |
|----------|-------------|
| `LINTHIS_CONFIG` | Override config file path |
| `LINTHIS_VERBOSE` | Enable verbose output |
| `LINTHIS_NO_COLOR` | Disable colored output |
