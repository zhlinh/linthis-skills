---
name: linthis.check
description: Run lint check on the codebase
allowed-tools:
  - Bash
  - Read
  - Glob
---

# /linthis.check

Run linthis lint check on the current project.

## Usage

```
/linthis.check [path...]
```

## Arguments

- `path...` - Optional paths to check. Defaults to current directory.

## Workflow

1. Check if linthis is installed
2. Check for .linthis/config.toml configuration
3. Run `linthis -c` on specified paths
4. Parse and display results
5. Suggest fixes if issues found

## Execution

```bash
# Check linthis installation
if ! command -v linthis &> /dev/null; then
    echo "linthis not found. Please install it first."
    exit 1
fi

# Run lint check
linthis -c $ARGS

# Show human-readable report if issues found
if [ $? -ne 0 ]; then
    linthis report show
fi
```

## Expected Output

On success:
```
✓ Lint check passed! No issues found.
```

On failure:
```
✗ Found 3 errors and 2 warnings

src/main.cpp:42:5
  E [cpplint] Missing semicolon

src/utils.h:15:10
  W [cpplint] Unused variable 'temp'

Run `/linthis.fix` to auto-fix issues or `/linthis.fix-ai` for AI assistance.
```
