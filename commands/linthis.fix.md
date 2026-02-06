---
name: linthis.fix
description: Auto-fix lint issues using built-in fixers
allowed-tools:
  - Bash
  - Read
  - Edit
  - Glob
---

# /linthis.fix

Auto-fix lint issues using linthis built-in fixers.

## Usage

```
/linthis.fix [path...]
```

## Arguments

- `path...` - Optional paths to fix. Defaults to files with issues from last check.

## Workflow

1. Run lint check if no recent results
2. Apply automatic fixes using language-specific formatters
3. Re-check to verify fixes
4. Report results

## Execution

```bash
# Run lint check with auto-fix
linthis -c -f $ARGS

# Show what was fixed
if [ $? -eq 0 ]; then
    echo "✓ All issues fixed!"
else
    echo "Some issues remain. Consider /linthis.fix-ai for AI assistance."
    linthis report show --errors-only
fi
```

## What Gets Fixed

Built-in fixers can handle:
- **Formatting**: Indentation, spacing, line length
- **Imports**: Sorting, unused removal
- **Simple errors**: Missing semicolons, trailing whitespace
- **Style**: Quote consistency, naming (some)

## What Needs AI

Use `/linthis.fix-ai` for:
- Logic errors
- Complex refactoring
- Missing implementations
- Security issues
- Design problems

## Expected Output

```
Fixing 5 issues in 3 files...

✓ src/main.cpp: 2 issues fixed
✓ src/utils.h: 1 issue fixed
✓ tests/test.py: 2 issues fixed

Re-checking...
✓ All issues resolved!
```
