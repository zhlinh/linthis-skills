# Report Generation

## Available Formats

| Format | Command | Use Case |
|--------|---------|----------|
| Human-readable | `linthis report show` | Terminal review |
| JSON | `linthis report json` | CI/CD, tools |
| HTML | `linthis report html` | Sharing, archive |
| Markdown | `linthis report markdown` | Documentation |

## Show Command

```bash
# Show last result in readable format
linthis report show

# Limit number of issues
linthis report show -n 10

# Show errors only
linthis report show --errors-only

# Compact format (no code context)
linthis report show --compact

# Show specific result file
linthis report show .linthis/result/result-2024-01-15.json
```

### Output Example

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

  ─────────────────────────────────────────────────

  src/utils.h:15:10
  ─────────────────────────────────────────────────
  W [cpplint] Unused variable 'temp'

    14 │ void process() {
    15 │     int temp = 0;  ← HERE
    16 │     // TODO: implement
    17 │ }

═══════════════════════════════════════════════════
  Summary: 1 error, 1 warning in 2 files
═══════════════════════════════════════════════════
```

## HTML Report

```bash
# Generate HTML report
linthis report html

# Open in browser automatically
linthis report html --open

# Custom output path
linthis report html -o report.html
```

### HTML Features
- Syntax-highlighted code
- Collapsible file sections
- Issue severity filtering
- Trend charts (with history)
- Search functionality

## Markdown Report

```bash
# Generate Markdown report
linthis report markdown

# Custom output
linthis report markdown -o LINT_REPORT.md
```

## JSON Output

```bash
# Output JSON to stdout
linthis report json

# Save to file
linthis report json -o results.json
```

### JSON Structure

```json
{
  "timestamp": "2024-01-15T10:30:45Z",
  "summary": {
    "total_files": 10,
    "files_with_issues": 2,
    "errors": 1,
    "warnings": 1,
    "info": 0
  },
  "issues": [
    {
      "file": "src/main.cpp",
      "line": 42,
      "column": 5,
      "severity": "error",
      "rule": "cpplint/semicolon",
      "message": "Missing semicolon at end of statement",
      "source": "    return x + y"
    }
  ]
}
```

## Report Configuration

```toml
# .linthis/config.toml
[reports]
# Default report format
default_format = "json"

# Include trend charts in HTML
include_trends = true

# Keep report history for N days
history_days = 30

# Result directory
result_dir = ".linthis/result"
```

## Result Files

Results are saved to `.linthis/result/`:
- `result-YYYY-MM-DD-HHMMSS.json` - Timestamped results
- `latest.json` - Symlink to most recent

```bash
# View saved results
ls .linthis/result/

# Show specific result
linthis report show .linthis/result/result-2024-01-15-103045.json
```
