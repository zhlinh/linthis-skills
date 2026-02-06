# Git Hooks Integration

## Installing Hooks

```bash
# Install pre-commit hook
linthis hook install

# Uninstall hook
linthis hook uninstall
```

## Hook Configuration

```toml
# .linthis/config.toml
[hooks]
# Enable pre-commit hook
pre_commit = true

# Exit on first error (fail fast)
exit_on_error = true

# Output width (0 = auto-detect terminal width)
output_width = 0
```

## Pre-commit Hook Behavior

When you run `git commit`, the hook:
1. Detects staged files
2. Runs linthis on staged files only
3. Shows compact issue summary
4. Blocks commit if errors found

### Hook Output Example

```
╭──────────────────────────────────────────────────╮
│ linthis Pre-commit Check                         │
├──────────────────────────────────────────────────┤
│ ✗ E src/main.cpp:42 Missing semicolon            │
│ ✗ E src/utils.h:15 Unused variable 'temp'        │
│ ! W tests/test.py:8 Line too long (120 > 100)    │
├──────────────────────────────────────────────────┤
│ 2 errors, 1 warning                              │
│ Commit blocked. Run 'linthis report show' for    │
│ details.                                         │
╰──────────────────────────────────────────────────╯
```

## Manual Hook Mode

Run linthis in hook mode manually:

```bash
# Pre-commit mode
linthis --hook-mode=pre-commit -c

# With auto-fix (careful!)
linthis --hook-mode=pre-commit -c -f
```

## Custom Hook Script

The installed hook is at `.git/hooks/pre-commit`:

```bash
#!/bin/sh
exec linthis --hook-mode=pre-commit -c
```

## CI/CD Integration

For CI pipelines, use quiet mode:

```bash
# Exit code indicates success/failure
linthis -c -q
echo $?  # 0 = pass, 1 = errors, 2 = format issues, 3 = warnings only
```

### GitHub Actions Example

```yaml
- name: Lint Check
  run: |
    linthis -c -q
    if [ $? -ne 0 ]; then
      linthis report show --errors-only
      exit 1
    fi
```

## Bypassing Hooks

In emergencies, bypass the hook:

```bash
git commit --no-verify -m "emergency fix"
```

**Warning**: Use sparingly. The hook exists to maintain code quality.
