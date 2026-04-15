# Daily Schedule Tracker — Claude CoWork Plugin

Track your day through natural dictation. Log activities, sync to Google Sheets, review weekly patterns, and see where your time goes.

## Commands

| Command | Description |
|---------|-------------|
| `/log` | Dictate your day — parsed into structured time entries |
| `/sync` | Sync confirmed entries to your Google Sheet |
| `/review` | Weekly review with targets vs actuals |
| `/report` | Daily summary with revenue focus |

## Features

- **Natural language parsing** — dictate freely, get structured entries
- **1,088 curated activities** across 19 categories from 20 research sources
- **Revenue attribution** — every hour tagged as Earning, Building, Seeding, Internal, or N/A
- **Project matching** — auto-detect which project you're working on
- **Energy tracking** — infer energy levels from activity context
- **Habit tracking** — weekly habit score with streak tracking
- **Gap detection** — flag unaccounted time > 15 minutes
- **Weekly reviews** — text-based charts, reflection prompts, week-over-week trends
- **Optional PowerShell watcher** — auto-track window focus in the background

## Install

### From folder (local)

1. Open Claude Desktop → CoWork
2. Go to Customize → Plugins → "+"
3. Select "Add from folder"
4. Navigate to this `plugin/` directory
5. Click Add

### From GitHub

1. Open Claude Desktop → CoWork
2. Go to Customize → Plugins → "+"
3. Select "Add from GitHub"
4. Enter: `1nicornbuilder/daily-schedule-tracker-plugin`
5. Click Add

### After install

Test with `/log` in your CoWork project. You'll need:
- Google Sheet shared with `sheet-writer@dailytracker-493320.iam.gserviceaccount.com`
- Service account JSON at `credentials/service-account.json`

## Requirements

- Claude Desktop with CoWork
- Google Sheet (template provided)
- Service account for Google Sheets API (gspread + google-auth)

## Author

Balaji Renukumar ([@1nicornbuilder](https://github.com/1nicornbuilder))

## License

MIT
