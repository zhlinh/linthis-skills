# AI Fix Loop

## How It Works

The AI fix loop automates the process of fixing lint issues iteratively:

```
┌─────────────┐
│ Lint Check  │
└──────┬──────┘
       │
       ▼
┌─────────────┐    No issues
│ Issues?     │─────────────────► Done
└──────┬──────┘
       │ Yes
       ▼
┌─────────────┐
│ Group by    │
│ File        │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Send to AI  │
│ with Context│
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Apply Fixes │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Re-check    │──► Loop back
│ Modified    │
└─────────────┘
```

## Running Fix Loop

### Interactive Mode

```bash
# Review each fix before applying
linthis fix --ai --provider claude-cli
```

Shows each suggested fix and prompts:
- `[y]` Accept fix
- `[n]` Skip fix
- `[a]` Accept all remaining
- `[q]` Quit

### Auto-Accept Mode

```bash
# Accept all fixes automatically
linthis fix --ai --provider claude-cli --accept-all
```

**Warning**: Use only when you trust the AI to make correct fixes.

## Loop Configuration

```toml
[ai]
# Maximum fix iterations (prevents infinite loops)
max_iterations = 100

# Stop if no progress after N iterations
stall_threshold = 3

# Re-check only modified files (faster)
recheck_modified_only = true
```

## Progress Display

During the fix loop:

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
  No issues found!

Fix loop completed: 3 issues fixed in 2 iterations
```

## Handling Complex Fixes

### Multi-File Issues

When an issue spans multiple files, linthis provides context:

```
Issue: Missing function definition
  Declared in: src/api.h:42
  Used in: src/main.cpp:15, src/utils.cpp:88

AI sees all related files for comprehensive fix.
```

### Context Window

```toml
[ai]
# Lines of context around each issue
context_lines = 10

# Include related files in context
include_related = true

# Maximum context size (tokens)
max_context_tokens = 8000
```

## Exit Conditions

The fix loop stops when:
1. No issues remain
2. Max iterations reached
3. No progress for `stall_threshold` iterations
4. User cancels (Ctrl+C)
5. AI fails to respond

## Best Practices

1. **Start Interactive**: Use interactive mode first to understand AI behavior
2. **Review First Run**: Check the first few fixes before enabling auto-accept
3. **Limit Scope**: Fix specific files rather than entire codebase
4. **Check Git Diff**: Review `git diff` after fix loop completes
5. **Test After**: Run tests to ensure fixes don't break functionality
