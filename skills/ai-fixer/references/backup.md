# Backup and Restore

## Automatic Backups

Linthis automatically creates backups before AI fixes:

```
.linthis/backups/
├── 2024-01-15-103045/
│   ├── manifest.json
│   ├── src/
│   │   ├── main.cpp
│   │   └── utils.h
│   └── tests/
│       └── test.py
└── 2024-01-15-092030/
    └── ...
```

## Configuration

```toml
[ai]
# Enable automatic backups
backup_enabled = true

# Backup directory
backup_dir = ".linthis/backups"

# Keep backups for N days
backup_retention_days = 7

# Maximum backup size (MB)
max_backup_size_mb = 100
```

## Managing Backups

### List Backups

```bash
linthis fix --list-backups
```

Output:
```
Available backups:
  1. 2024-01-15-103045 (5 files, 12.3 KB)
  2. 2024-01-15-092030 (3 files, 8.1 KB)
  3. 2024-01-14-163022 (10 files, 45.2 KB)
```

### Undo Last Fix

```bash
# Restore from most recent backup
linthis fix --undo
```

### Restore Specific Backup

```bash
# Restore from specific backup
linthis fix --restore 2024-01-15-092030
```

### Clean Old Backups

```bash
# Remove backups older than retention period
linthis fix --clean-backups
```

## Backup Manifest

Each backup includes a `manifest.json`:

```json
{
  "timestamp": "2024-01-15T10:30:45Z",
  "files": [
    {
      "path": "src/main.cpp",
      "original_hash": "abc123...",
      "size": 4096
    }
  ],
  "issues_fixed": 5,
  "provider": "claude-cli"
}
```

## Selective Restore

Restore specific files only:

```bash
# Restore single file
linthis fix --restore 2024-01-15-103045 --file src/main.cpp

# Restore directory
linthis fix --restore 2024-01-15-103045 --file src/
```

## Git Integration

Backups work alongside Git:

```bash
# Before AI fix
git stash  # Optional: save uncommitted changes

# Run AI fix
linthis fix --ai --provider claude-cli --accept-all

# Review changes
git diff

# If unhappy, restore
linthis fix --undo

# Or use git
git checkout -- .
```

## Disabling Backups

**Not recommended**, but possible:

```toml
[ai]
backup_enabled = false
```

Or per-command:
```bash
linthis fix --ai --provider claude-cli --no-backup
```

## Troubleshooting

### Backup Too Large

```
Error: Backup would exceed 100 MB limit
```

Solutions:
1. Increase limit: `max_backup_size_mb = 500`
2. Exclude large files in config
3. Fix smaller file sets

### Restore Failed

```
Error: File src/main.cpp has been modified since backup
```

Options:
1. Force restore: `--restore ... --force`
2. Manually merge changes
3. Use Git instead

### Disk Space

Monitor backup directory size:
```bash
du -sh .linthis/backups/
```

Clean old backups:
```bash
linthis fix --clean-backups
```
