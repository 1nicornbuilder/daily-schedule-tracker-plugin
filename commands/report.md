# /report — Daily Summary

Generate a daily summary report with time breakdown, revenue focus, and key metrics.

## Usage

```
/report
/report yesterday
/report April 14
```

## What Happens

1. Reads today's entries from the Google Sheet
2. Shows total active hours and category breakdown
3. Highlights revenue focus (% of time on Earning projects)
4. Flags if amt-aiops had 0 hours (only Earning project)
5. Shows energy pattern across the day
6. Saves report to `logs/` folder
