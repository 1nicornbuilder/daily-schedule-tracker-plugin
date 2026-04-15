---
name: sync-review
description: Review parsed time entries and save confirmed entries to the local session_log.csv for automatic Google Sheet sync
---

# Sync & Review — CSV Writer

You handle the review-confirm-save flow. After daily-log parses the user's dictation into a table, you present it for review, accept edits, and write confirmed entries to the local CSV.

**IMPORTANT: You CANNOT call Google Sheets API.** CoWork runs in a sandbox without external API access. You write to the local CSV file. A Windows Task Scheduler job on the user's machine automatically syncs CSV to Google Sheet at 9 AM, 2 PM, 8 PM, and 11:45 PM.

## Review Flow

1. **Present**: Show the parsed entries table (from daily-log skill)
2. **Confirm**: Wait for user to say "sync", "looks good", "send it", or similar
3. **Edit**: If user says "change row 3 to..." or "row 5 should be...", update and re-present
4. **Save**: On confirmation, append entries to the local CSV file

Never write to CSV without explicit user confirmation.

## Where to Write

**File**: `D:\Programming\daily-schedule-tracker\data\session_log.csv`

### CSV Format

```
date,session_start,session_end,duration_seconds,application,window_title,matched_project,matched_category,is_idle
```

### Column Mapping from Parsed Table

| CSV Column | Source |
|-----------|--------|
| date | Date (YYYY-MM-DD) |
| session_start | Begin time (HH:MM:SS, 24h format) |
| session_end | End time (HH:MM:SS, 24h format) |
| duration_seconds | (end - start) in seconds |
| application | Inferred app (e.g., "vscode", "chrome", "manual") — use "dictated" for manually dictated entries |
| window_title | Activity description |
| matched_project | Project name from projects.json |
| matched_category | Category from 19-category list |
| is_idle | False (dictated entries are never idle) |

### Example Rows

```csv
2026-04-15,10:15:00,10:45:00,1800,dictated,Played with kids outside,Personal / Non-work,Family / Kids,False
2026-04-15,11:00:00,11:15:00,900,dictated,Set up daily tracker,Daily Schedule Tracker,Deep Work,False
```

## Save Confirmation Message

After writing to CSV:

```
Saved [N] entries to local CSV.
Auto-sync to Google Sheet at next scheduled time (9 AM, 2 PM, 8 PM, or 11:45 PM).

To force an immediate sync, run in PowerShell:
  python "D:\Programming\daily-schedule-tracker\scripts\sync_engine.py"

Summary:
  Total: [X]h [Y]m logged
  Revenue: Earning [X]h | Building [X]h | Seeding [X]h | Internal [X]h
```

## Error Handling

- **CSV file missing**: Create it with the header row, then append entries
- **CSV file locked**: "The session log is currently locked by another process. Wait a moment and try again."
- **Duplicate entries warning**: If entries with the same date and start time already exist in CSV, warn: "Entries for [time] already exist. Append anyway?"

## Rules

- Never try to call Google Sheets API — you will get a 403 error
- Never write to CSV without user confirmation
- All timestamps in US Eastern (America/New_York)
- Use "dictated" as the application name for manually dictated entries (vs auto-tracked entries which use app names like "chrome", "vscode")
- Duration column is calculated as (end_time - start_time) in seconds
- Append to CSV, never overwrite existing rows
