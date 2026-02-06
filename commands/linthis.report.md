---
name: linthis.report
description: View and generate lint reports
allowed-tools:
  - Bash
  - Read
  - Glob
---

# /linthis.report

View lint results and generate reports in various formats.

## Usage

```
/linthis.report [show|html|markdown|json] [options]
```

## Subcommands

### show (default)

Display last lint results in human-readable format.

```
/linthis.report show
/linthis.report show -n 10           # Limit to 10 issues
/linthis.report show --errors-only   # Only errors
/linthis.report show --compact       # No code context
```

### html

Generate HTML report with syntax highlighting.

```
/linthis.report html
/linthis.report html --open          # Open in browser
/linthis.report html -o report.html  # Custom output path
```

### markdown

Generate Markdown report for documentation.

```
/linthis.report markdown
/linthis.report markdown -o LINT.md
```

### json

Output raw JSON for tooling integration.

```
/linthis.report json
/linthis.report json -o results.json
```

## Workflow

1. Check for existing lint results in `.linthis/result/`
2. If no results, run `linthis -c` first
3. Generate requested report format
4. Display or save output

## Execution

```bash
# Show human-readable report
case "$SUBCOMMAND" in
    show)
        linthis report show $ARGS
        ;;
    html)
        linthis report html $ARGS
        ;;
    markdown)
        linthis report markdown $ARGS
        ;;
    json)
        linthis report json $ARGS
        ;;
    *)
        linthis report show $ARGS
        ;;
esac
```

## Expected Output (show)

```
═══════════════════════════════════════════════════
  Lint Report - 2024-01-15 10:30:45
═══════════════════════════════════════════════════

  src/main.cpp:42:5
  ─────────────────────────────────────────────────
  E [cpplint] Missing semicolon at end of statement

    40 │     int x = 10
    41 │     int y = 20
    42 │     return x + y  ← HERE
    43 │ }

═══════════════════════════════════════════════════
  Summary: 1 error in 1 file
═══════════════════════════════════════════════════
```
