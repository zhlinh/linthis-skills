---
name: linthis.format
description: Format code using language-specific formatters
allowed-tools:
  - Bash
  - Read
  - Glob
---

# /linthis.format

Format code using language-specific formatters (clang-format, ruff, prettier, gofmt, rustfmt, etc.).

## Usage

```
/linthis.format [--check] [path...]
```

## Arguments

- `--check` - Check formatting without modifying files (dry-run)
- `path...` - Optional paths to format. Defaults to current directory.

## Workflow

1. Detect languages in specified paths
2. Run appropriate formatter for each language
3. Report formatting changes
4. Optionally show diff of changes

## Execution

```bash
# Check if formatting is needed (dry-run)
if [[ "$ARGS" == *"--check"* ]]; then
    linthis format --check $PATHS
    if [ $? -ne 0 ]; then
        echo "✗ Formatting issues found"
        echo "  Run /linthis.format to fix them"
        exit 2
    else
        echo "✓ All files properly formatted"
    fi
else
    # Apply formatting
    linthis format $PATHS
    echo "✓ Formatting complete"
fi
```

## Formatters by Language

| Language | Formatter | Config File |
|----------|-----------|-------------|
| C++ | clang-format | .clang-format |
| Python | ruff / black | pyproject.toml |
| JavaScript | prettier | .prettierrc |
| TypeScript | prettier | .prettierrc |
| Go | gofmt | - |
| Rust | rustfmt | rustfmt.toml |
| Java | google-java-format | - |
| Kotlin | ktlint | .editorconfig |
| Swift | swiftformat | .swiftformat |

## Configuration

```toml
# .linthis/config.toml
[languages.cpp]
formatter = "clang-format"

[languages.python]
formatter = "ruff"  # or "black", "autopep8"

[languages.javascript]
formatter = "prettier"

[languages.go]
formatter = "gofmt"

[languages.rust]
formatter = "rustfmt"
```

## Expected Output

### Format Mode

```
Formatting 5 files...

✓ src/main.cpp (reformatted)
✓ src/utils.cpp (reformatted)
  src/api.h (no changes)
✓ tests/test.py (reformatted)
  README.md (no changes)

3 files reformatted, 2 files unchanged
```

### Check Mode

```
Checking formatting...

✗ src/main.cpp
  - Line 42: Expected 4 spaces, found 2
  - Line 58: Trailing whitespace

✗ tests/test.py
  - Line 15: Line too long (125 > 100)

2 files need formatting
Run /linthis.format to fix
```

## Formatter-Specific Options

### clang-format

```toml
[languages.cpp]
formatter = "clang-format"
formatter_args = ["--style=Google"]
```

### ruff

```toml
[languages.python]
formatter = "ruff"
line_length = 100
```

### prettier

```toml
[languages.javascript]
formatter = "prettier"
formatter_args = ["--single-quote", "--trailing-comma=es5"]
```

## Integration with Check

Format is automatically included in lint check with `-f`:

```bash
# Check + auto-format
linthis -c -f

# Equivalent to:
linthis format && linthis -c
```

## Pre-commit Hook

Enable format checking in pre-commit:

```toml
[hooks]
pre_commit = true
format_check = true  # Block commit if not formatted
```
