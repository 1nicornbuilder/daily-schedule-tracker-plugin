---
name: weekly-review
description: Generate weekly time allocation review with targets vs actuals, revenue focus, project breakdown, habit scores, and reflection prompts
---

# Weekly Review — Aggregation & Reflection

You generate a comprehensive weekly review by reading the Google Sheet data for the past 7 days. Present time allocation, revenue focus, project breakdown, habit scores, and reflection prompts.

## Data Source

Read from Google Sheet ID `1nZ9YBCHWB2dkptKK4mo4A4GiMSM_gB5YfE6gLvZGBg4`:
- **Daily Time Log** tab: all entries for the week (Monday–Sunday)
- **Daily Dashboard** tab: daily summaries and ratings
- **Habit Tracker** tab: habit completion data
- **Weekly Summary** tab: write the review here

## Weekly Review Output

### 1. Time Allocation — Targets vs Actuals

Present text-based bar charts showing target vs actual hours per category:

```
WEEKLY TIME ALLOCATION (Mon [date] – Sun [date])
═══════════════════════════════════════════════

Deep Work        ████████████████░░░░  32h target | 28h actual (88%)
BD / Sales       ████████░░░░░░░░░░░░   8h target |  6h actual (75%)
Content Creation ████░░░░░░░░░░░░░░░░   4h target |  3h actual (75%)
Strategy         ████░░░░░░░░░░░░░░░░   4h target |  5h actual (125%)
Meetings / Calls ████░░░░░░░░░░░░░░░░   4h target |  3h actual (75%)
Admin            ████░░░░░░░░░░░░░░░░   4h target |  6h actual (150%) ⚠
Fitness          ██████░░░░░░░░░░░░░░   5h target |  4h actual (80%)
Family / Kids    ████████░░░░░░░░░░░░  10h target | 12h actual (120%)
Personal         ████░░░░░░░░░░░░░░░░   4h target |  3h actual (75%)
Sleep            ██████████████████░░  49h target | 52h actual (106%)
```

Flag categories over 120% or under 60% of target with ⚠.

Weekly targets (default, adjustable in Settings tab):
- Deep Work: 32h | BD/Sales: 8h | Content: 4h | Strategy: 4h
- Meetings: 4h | Admin: 4h | Fitness: 5h | Family: 10h
- Personal: 4h | Sleep: 49h (7h/night)

### 2. Revenue Focus Breakdown

```
REVENUE FOCUS — WEEK OF [date]
═══════════════════════════════

Earning   ████████████████████  18h (36%) — amt-aiops
Building  ████████████░░░░░░░░  10h (20%) — NeverMissAFiling (4h), BayCare (3h), TPA (3h)
Seeding   ████████░░░░░░░░░░░░   6h (12%) — 1nicorn/Substack (4h), cmail (2h)
Internal  ██████░░░░░░░░░░░░░░   5h (10%) — Daily Tracker (3h), CC General (2h)
N/A       ██████████░░░░░░░░░░  11h (22%) — Personal, Family, Sleep excluded

Week-over-week: Earning ↑3h | Building ↓2h | Seeding →0h
```

⚠ Flag if Earning < 15h in a work week.

### 3. Top 5 Projects by Hours

```
TOP 5 PROJECTS
═══════════════
1. amt-aiops (Crane Monitoring)     18h  [Earning]
2. NeverMissAFiling                  4h  [Building]
3. 1nicorn (Substack)                4h  [Seeding]
4. BayCare (BD)                      3h  [Building]
5. Daily Schedule Tracker            3h  [Internal]
```

### 4. Habit Score

Read from the Habit Tracker tab. Calculate completion percentage:

```
HABIT TRACKER — WEEK SCORE
═══════════════════════════
Morning routine    ✓✓✓✓✓✓✗  6/7  86%
Exercise           ✓✓✓✗✓✓✗  5/7  71%
Journaling         ✓✓✗✗✓✗✗  3/7  43% ⚠
Meditation         ✓✓✓✓✓✓✓  7/7  100% ★
Bed by 11 PM       ✓✓✗✓✓✗✓  5/7  71%
────────────────────────────────────
Overall Score:                74%
Last week:                    68% (↑6%)
```

Flag habits under 50% with ⚠. Star habits at 100% with ★.

### 5. Reflection Prompts

Present these prompts and ask the user to respond:

```
WEEKLY REFLECTION
═════════════════

1. Benjamin Franklin: "What good did I do this week?"
   →

2. Revenue Focus: "Did I spend enough time on Earning (amt-aiops)?
   What blocked me from more revenue hours?"
   →

3. Energy Audit: "Which day had the best energy? What made it different?"
   →

4. Day Ratings: "Average rating this week: [X]/10.
   Best day: [day] ([rating]). Worst day: [day] ([rating]).
   What pattern do you see?"
   →

5. Next Week: "One thing to do more of, one thing to do less of."
   →
```

After the user responds, save their reflections to the Weekly Summary tab.

## Write to Weekly Summary Tab

After generating the review:
1. Find or create the row for this week (by week start date)
2. Write: total hours, category breakdown, revenue breakdown, top project, habit score, day ratings average
3. Save reflection responses in the Notes column

## Rules

- All times in US Eastern (America/New_York)
- Read data from Sheet (source of truth), not local CSV
- Present the full review before asking for reflections
- Flag amt-aiops if < 15h/week or 0h on any workday
- Use text-based charts (no images) — must render in CoWork text interface
- Compare to previous week when data is available
