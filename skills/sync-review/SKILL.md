---
name: sync-review
description: Review parsed time entries and sync confirmed entries to Google Sheets via gspread and service account
---

# Sync & Review — Google Sheet Writer

You handle the review-confirm-sync flow. After daily-log parses the user's dictation into a table, you present it for review, accept edits, and write confirmed entries to the Google Sheet.

## Review Flow

1. **Present**: Show the parsed entries table (from daily-log skill)
2. **Confirm**: Wait for user to say "sync", "looks good", "send it", or similar
3. **Edit**: If user says "change row 3 to..." or "row 5 should be...", update and re-present
4. **Sync**: On confirmation, write to Google Sheet

Never write to the Sheet without explicit user confirmation.

## Google Sheet Connection

### Requirements
- Python library: `gspread` + `google-auth`
- Service account JSON: `D:\Programming\daily-schedule-tracker\credentials\service-account.json`
- Google Sheet ID: `1nZ9YBCHWB2dkptKK4mo4A4GiMSM_gB5YfE6gLvZGBg4`

### First-Time Setup

If the Google Sheet connection is not configured or credentials are missing, guide the user:

```
To connect your Google Sheet:

1. Ensure the service account JSON exists at:
   D:\Programming\daily-schedule-tracker\credentials\service-account.json

2. Share your Google Sheet with the service account email:
   sheet-writer@dailytracker-493320.iam.gserviceaccount.com
   (Editor access)

3. The Sheet ID is configured as: 1nZ9YBCHWB2dkptKK4mo4A4GiMSM_gB5YfE6gLvZGBg4

Once set up, try /sync again.
```

### Connection Code Pattern

```python
import gspread
from google.oauth2.service_account import Credentials

SCOPES = [
    "https://www.googleapis.com/auth/spreadsheets",
    "https://www.googleapis.com/auth/drive"
]

creds = Credentials.from_service_account_file(
    r"D:\Programming\daily-schedule-tracker\credentials\service-account.json",
    scopes=SCOPES
)
gc = gspread.authorize(creds)
sh = gc.open_by_key("1nZ9YBCHWB2dkptKK4mo4A4GiMSM_gB5YfE6gLvZGBg4")
```

## Write Targets

### 1. Daily Time Log (append rows)

Append each confirmed entry as a new row with these columns:
| Column | Field |
|--------|-------|
| A | Date (MM/DD/YYYY) |
| B | Begin Time (h:mm AM/PM) |
| C | End Time (h:mm AM/PM) |
| D | Duration (formula: =C-B) |
| E | Activity Description |
| F | Category (from 19 categories) |
| G | Subcategory |
| H | Project |
| I | Energy Level (1-5) |
| J | Revenue Horizon (Earning/Building/Seeding/Internal/N/A) |
| K | Work Mode |
| L | Notes |

Use `worksheet.append_rows()` for batch writing (faster than individual append_row calls).

### 2. Daily Dashboard (update date row)

Find or create the row for today's date in the Daily Dashboard tab. Update:
- Total Hours (sum of durations)
- Category breakdown (hours per category)
- Top project
- Revenue hours breakdown
- Day rating (ask user at end of sync)

### 3. Charts & Dashboards Data Tables

Update the source data tables that feed the 8 charts:
- Category Distribution table
- Daily Hours Trend table
- Revenue Focus table
- Project Hours table

These tables auto-refresh the charts when data changes.

## Sync Confirmation Message

After successful sync, display:

```
✓ Synced [N] entries to Google Sheet
  → Daily Time Log: [N] rows appended (rows XX-YY)
  → Daily Dashboard: [date] row updated
  → Total: [X]h [Y]m logged

  Revenue breakdown:
  • Earning: Xh Ym
  • Building: Xh Ym
  • Seeding: Xh Ym
  • Internal: Xh Ym
```

## Error Handling

- **No credentials**: Show first-time setup guide (above)
- **Sheet not shared**: "The Google Sheet isn't shared with the service account. Share it with sheet-writer@dailytracker-493320.iam.gserviceaccount.com"
- **API rate limit**: "Google Sheets API rate limit hit. Wait 60 seconds and try again."
- **Network error**: "Can't reach Google Sheets. Check your internet connection and try /sync again."
- **Duplicate date warning**: "Entries for [date] already exist in the Sheet. Append anyway? (This won't overwrite existing rows.)"

## Rules

- Never overwrite existing Sheet data — always append
- Never sync without user confirmation
- Use batch operations to minimize API calls (gspread rate limit: 60 req/min)
- All timestamps in US Eastern (America/New_York)
- Duration column uses a formula (=C-B), not a hardcoded value
