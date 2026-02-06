# Linthis Configuration Guide

## Configuration File Location

Linthis looks for configuration in:
1. `.linthis/config.toml` (project-specific)
2. `~/.config/linthis/config.toml` (user-wide)
3. Command-line arguments (highest priority)

## Full Configuration Example

```toml
# .linthis/config.toml

[general]
# Enable verbose output
verbose = false

# Patterns to exclude from linting
exclude = [
    "vendor/**",
    "node_modules/**",
    "*.pb.go",
    "**/generated/**",
    "third_party/**"
]

# Number of parallel jobs (0 = auto)
jobs = 0

# Cache directory
cache_dir = ".linthis/cache"

# Result directory
result_dir = ".linthis/result"

[languages.cpp]
enabled = true
standard = "c++17"  # c++11, c++14, c++17, c++20
linter = "cpplint"  # cpplint, clang-tidy
formatter = "clang-format"
extra_args = [
    "--filter=-whitespace/line_length,-build/include_subdir"
]

[languages.python]
enabled = true
linter = "ruff"  # ruff, pylint, flake8
formatter = "ruff"  # ruff, black, autopep8

[languages.javascript]
enabled = true
linter = "eslint"
formatter = "prettier"

[languages.typescript]
enabled = true
linter = "eslint"
formatter = "prettier"

[languages.go]
enabled = true
linter = "golangci-lint"
formatter = "gofmt"

[languages.rust]
enabled = true
linter = "clippy"
formatter = "rustfmt"

[languages.java]
enabled = true
linter = "checkstyle"
formatter = "google-java-format"

[languages.kotlin]
enabled = true
linter = "ktlint"
formatter = "ktlint"

[languages.swift]
enabled = true
linter = "swiftlint"
formatter = "swiftformat"

[hooks]
# Enable pre-commit hook
pre_commit = true

# Exit on first error in hook mode
exit_on_error = true

# Hook output width (0 = auto)
output_width = 0

[reports]
# Default report format
default_format = "json"

# Include trends in HTML report
include_trends = true

# Keep report history for N days
history_days = 30
```

## Language-Specific Settings

### C++ Configuration
```toml
[languages.cpp]
enabled = true
standard = "c++17"
linter = "cpplint"
extra_args = [
    "--filter=-whitespace/line_length",
    "--filter=-build/include_subdir",
    "--filter=-readability/todo"
]
include_dirs = ["include", "src"]
```

### Python Configuration
```toml
[languages.python]
enabled = true
linter = "ruff"
formatter = "ruff"
line_length = 100
target_version = "py311"
```

### TypeScript/JavaScript Configuration
```toml
[languages.typescript]
enabled = true
linter = "eslint"
formatter = "prettier"
config_file = ".eslintrc.js"
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `LINTHIS_CONFIG` | Path to config file |
| `LINTHIS_CACHE_DIR` | Cache directory |
| `LINTHIS_NO_COLOR` | Disable colored output |
| `LINTHIS_VERBOSE` | Enable verbose mode |

## Plugin Configuration

```toml
[plugins]
# Enable plugins from a directory
plugin_dir = ".linthis/plugins"

# Specific plugin configurations
[plugins.custom-checker]
enabled = true
command = "python check.py"
languages = ["python"]
```
