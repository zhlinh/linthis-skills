---
name: linthis.fix-ai
description: Fix lint issues with AI assistance
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Glob
---

# /linthis.fix-ai

Fix lint issues using AI-powered code remediation.

## Usage

```
/linthis.fix-ai [--accept-all] [--provider <provider>] [path...]
```

## Arguments

- `--accept-all` - Auto-accept all AI suggestions (use with caution)
- `--provider <provider>` - AI provider (claude-cli, claude, codebuddy-cli, local)
- `path...` - Optional paths to fix. Defaults to files with issues.

## Workflow

1. Run lint check to identify issues
2. Create backup of affected files
3. Send issues with context to AI
4. Apply AI-suggested fixes
5. Re-check modified files
6. Repeat until no issues or max iterations

## Execution

```bash
# Check for issues first
linthis -c $PATHS

# Run AI fix with default provider
linthis fix --ai --provider claude-cli $ARGS

# Show results
linthis report show
```

## AI Providers

| Provider | Flag | Notes |
|----------|------|-------|
| Claude CLI | `--provider claude-cli` | Recommended, local Claude |
| Claude API | `--provider claude` | Requires ANTHROPIC_API_KEY |
| CodeBuddy | `--provider codebuddy-cli` | Tencent CodeBuddy |

## Safety Features

- **Automatic backups**: Before any AI fix
- **Iteration limit**: Max 100 iterations by default
- **Modified-only recheck**: Only re-checks changed files
- **Undo support**: `linthis fix --undo`

## Expected Output

```
⠋ Running Claude CLI... (1m 23s)

Iteration 1/100:
  ✓ Fixed src/main.cpp (2 issues)
  ✓ Fixed src/utils.h (1 issue)

Re-checking 2 modified files...
  Found 1 remaining issue

Iteration 2/100:
  ✓ Fixed src/main.cpp (1 issue)

Re-checking 1 modified file...
  ✓ No issues found!

Fix completed: 3 issues fixed in 2 iterations
Backup saved to: .linthis/backups/2024-01-15-103045/
```

## Rollback

If AI fixes cause problems:

```bash
# Undo last fix
linthis fix --undo

# Or restore specific backup
linthis fix --restore 2024-01-15-103045
```
