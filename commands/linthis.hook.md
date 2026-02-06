---
name: linthis.hook
description: Manage Git hooks for pre-commit linting
allowed-tools:
  - Bash
  - Read
  - Write
  - Glob
---

# /linthis.hook

Install and manage Git pre-commit hooks for automatic linting.

## Usage

```
/linthis.hook [install|uninstall|status]
```

## Subcommands

### install

Install the pre-commit hook.

```
/linthis.hook install
```

Creates `.git/hooks/pre-commit` that runs linthis on staged files.

### uninstall

Remove the pre-commit hook.

```
/linthis.hook uninstall
```

### status

Check current hook status.

```
/linthis.hook status
```

## Workflow

1. Verify `.git` directory exists
2. Check current hook status
3. Install/uninstall as requested
4. Verify hook is executable

## Execution

```bash
case "$SUBCOMMAND" in
    install)
        linthis hook install
        echo "✓ Pre-commit hook installed"
        echo "  Staged files will be checked before each commit."
        ;;
    uninstall)
        linthis hook uninstall
        echo "✓ Pre-commit hook removed"
        ;;
    status)
        if [ -f .git/hooks/pre-commit ] && grep -q "linthis" .git/hooks/pre-commit; then
            echo "✓ Linthis hook is installed"
        else
            echo "✗ Linthis hook is not installed"
            echo "  Run /linthis.hook install to enable pre-commit checking"
        fi
        ;;
    *)
        echo "Usage: /linthis.hook [install|uninstall|status]"
        ;;
esac
```

## Hook Behavior

When you run `git commit`:

1. Hook detects staged files
2. Runs `linthis --hook-mode=pre-commit -c`
3. Shows compact issue summary
4. **Blocks commit** if errors found
5. Allows commit if only warnings

## Hook Output

```
╭──────────────────────────────────────────────────╮
│ linthis Pre-commit Check                         │
├──────────────────────────────────────────────────┤
│ ✗ E src/main.cpp:42 Missing semicolon            │
│ ! W tests/test.py:8 Line too long (120 > 100)    │
├──────────────────────────────────────────────────┤
│ 1 error, 1 warning - Commit blocked              │
│ Run 'linthis report show' for details            │
╰──────────────────────────────────────────────────╯
```

## Bypassing Hook

In emergencies:

```bash
git commit --no-verify -m "emergency fix"
```

**Warning**: Use sparingly!
