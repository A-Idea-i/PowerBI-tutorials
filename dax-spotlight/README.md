# Power BI DAX Spotlight

Five short, standalone Power BI tutorials, each focused on one DAX idea. Every video has its own tiny sample dataset, so you can watch them in any order.

> **Playlist:** [Power BI DAX Spotlight on YouTube](https://www.youtube.com/playlist?list=PLek1t11USgEU)

## What you'll learn

| # | Video | DAX covered | Data file |
|---|-------|-------------|-----------|
| 1 | [CALENDARAUTO(): build a date table in one line](https://youtu.be/YmWYcDxKMD0) | `CALENDARAUTO`, `YEAR`, `MONTH`, `FORMAT`, `ROUNDUP` | `data/Events.csv` |
| 2 | [Time intelligence: YTD, last year and growth %](https://youtu.be/L8xyKaXqMNc) | `TOTALYTD`, `SAMEPERIODLASTYEAR`, `DATEADD`, `DIVIDE`, `CALCULATE` | `data/Subscriptions.csv` |
| 3 | [SWITCH(TRUE()): say goodbye to nested IFs](https://youtu.be/vmFwas5vwy4) | `SWITCH`, `TRUE`, `IF`, `FILTER`, `COUNTROWS` | `data/Tickets.csv` |
| 4 | [ALL vs ALLSELECTED: percent of total and running total](https://youtu.be/PePnbGaJWpM) | `ALL`, `ALLSELECTED`, `CALCULATE`, `FILTER`, `MAX` | `data/Traffic.csv` |
| 5 | [RANKX, TOPN and dynamic titles](https://youtu.be/mDIUAMrTFEA) | `RANKX`, `TOPN`, `SELECTEDVALUE`, `ALL` | `data/Reps.csv` |

## Repository structure

```
dax-spotlight/
├── README.md
├── LICENSE
├── data/
│   ├── Events.csv
│   ├── Subscriptions.csv
│   ├── Tickets.csv
│   ├── Traffic.csv
│   └── Reps.csv
└── pbix/                      (optional finished reports)
    ├── DAX_Video1_CALENDARAUTO.pbix
    ├── DAX_Video2_TimeIntelligence.pbix
    ├── DAX_Video3_SWITCH.pbix
    ├── DAX_Video4_ALLSELECTED.pbix
    └── DAX_Video5_RANKX.pbix
```

## How to follow along

1. Download the CSV for the video you are watching from the `data/` folder (open the file on GitHub, then click **Download raw file**).
2. Open a **blank** report in Power BI Desktop.
3. On the **Home** tab, click **Get data**, choose **Text/CSV**, select the file, and click **Load**.
4. In **Table view**, check each column's data type under the **Column tools** tab (set date columns to **Date**).
5. Create tables, columns and measures with **New table**, **New column** and **New measure** on the **Home** tab, and type the formulas from the video.

**Tip for Videos 1 and 2:** turn off Auto date/time for the file under **File > Options and settings > Options > Current File > Data Load**, so Power BI's hidden date hierarchies don't get in the way of your own Calendar table. This applies to the open report only. To turn it off for every new report, untick **Auto date/time for new files** under **Global > Data Load**.

## Datasets and expected results

Use these figures to check that your formulas are correct.

### Video 1: Events

| Column | Type |
|--------|------|
| Event | Text |
| Event Date | Date |
| Attendees | Whole number |

- 7 events, 1,030 total attendees.
- `CALENDARAUTO()` returns **730 days**, from 1 January 2025 to 31 December 2026.
- `CALENDARAUTO(3)` (fiscal year ending March) runs from 1 April 2025 to 31 March 2027.

### Video 2: Subscriptions

| Column | Type |
|--------|------|
| Month Start | Date |
| Revenue | Whole number |

**Calendar setup (five steps):**

1. New table: `Calendar = CALENDARAUTO()` (730 rows, 1 January 2025 to 31 December 2026).
2. New columns: `Year`, `Month Number`, `Month Name`.
3. Sort `Month Name` by `Month Number` (Column tools > Sort by column).
4. Table tools > **Mark as date table**, using the `Date` column.
5. Model view: drag `Subscriptions[Month Start]` onto `Calendar[Date]` (one-to-many, with the 1 on the Calendar side).

For **June 2026** you should see:

| Total Revenue | Revenue Last Year | YoY % | Revenue YTD |
|---------------|-------------------|-------|-------------|
| 1,600 | 1,250 | 28% | 8,700 |

For March 2026: Total Revenue 1,500, Revenue Last Year 1,200, YoY % 25%.

### Video 3: Tickets

| Column | Type |
|--------|------|
| Ticket | Text |
| Hours | Whole number |
| Priority | Text |

- `Speed Band` is a **calculated column**: **3 Fast, 4 On Time, 3 Late**.
- `Late %` is a **measure**, shown on a Card: **30%**.
- `Action` is a **calculated column**: tickets flagged "Escalate" are **T007** and **T009**.

### Video 4: Traffic

| Column | Type |
|--------|------|
| Month Start | Date |
| Channel | Text |
| Visits | Whole number |

All channels (no slicer selection):

| Channel | Visits | % of Total |
|---------|--------|------------|
| Search | 1,650 | 47.1% |
| Social | 900 | 25.7% |
| Email | 370 | 10.6% |
| Direct | 580 | 16.6% |

With only **Search** and **Social** selected in the slicer:

| Channel | Visits | % of Total (ALL) | % of Selected (ALLSELECTED) |
|---------|--------|------------------|-----------------------------|
| Search | 1,650 | 47.1% | 64.7% |
| Social | 900 | 25.7% | 35.3% |
| Total row | 2,550 | 72.9% | 100% |

Denominators for Search with Search and Social selected: no function 1,650, `ALLSELECTED` 2,550, `ALL` 3,500.

Running total (no slicer): January 1,100, February 2,300, March 3,500. With only Search selected: 500, 1,050, 1,650.

### Video 5: Reps

| Column | Type |
|--------|------|
| Rep | Text |
| Region | Text |
| Deals | Whole number |

Total deals: **124**.

| Rep | Total Deals | Rank | Rank Dense |
|-----|-------------|------|------------|
| Farid | 25 | 1 | 1 |
| Chloe | 21 | 2 | 2 |
| Dev | 17 | 3 | 3 |
| Hiro | 17 | 3 | 3 |
| Ana | 14 | 5 | 4 |
| Grace | 12 | 6 | 5 |
| Ben | 9 | 7 | 6 |
| Elena | 9 | 7 | 6 |

- `Top 3 Deals` = **80**, because `TOPN` keeps ties (Farid, Chloe, Dev and Hiro).
- `Top 3 Deals (strict)` = **63**, because the alphabetical tie-breaker keeps only Farid, Chloe and Dev.

| Region slicer | Title shown | Deals |
|---------------|-------------|-------|
| Nothing selected | Deals closed: All regions | 124 |
| East | Deals closed: East | 46 |
| North | Deals closed: North | 31 |
| North and West | Deals closed: All regions | 57 |

## Formulas used

<details>
<summary>Video 1: CALENDARAUTO()</summary>

```dax
// New table
Calendar = CALENDARAUTO()
Calendar FY = CALENDARAUTO(3)

// New columns
Year = YEAR('Calendar'[Date])
Month Number = MONTH('Calendar'[Date])
Month Name = FORMAT('Calendar'[Date], "MMM")
Quarter = "Q" & ROUNDUP(MONTH('Calendar'[Date]) / 3, 0)
```
</details>

<details>
<summary>Video 2: Time intelligence</summary>

```dax
// New table
Calendar = CALENDARAUTO()

// New columns
Year = YEAR('Calendar'[Date])
Month Number = MONTH('Calendar'[Date])
Month Name = FORMAT('Calendar'[Date], "MMM")

// New measures
Total Revenue = SUM(Subscriptions[Revenue])
Revenue YTD = TOTALYTD([Total Revenue], 'Calendar'[Date])
Revenue Last Year = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Calendar'[Date]))
YoY % = DIVIDE([Total Revenue] - [Revenue Last Year], [Revenue Last Year])
```
</details>

<details>
<summary>Video 3: SWITCH(TRUE())</summary>

```dax
// New columns
Speed Band (IF) = IF(Tickets[Hours] <= 4, "Fast", IF(Tickets[Hours] <= 24, "On Time", "Late"))
Speed Band = SWITCH(TRUE(), Tickets[Hours] <= 4, "Fast", Tickets[Hours] <= 24, "On Time", "Late")
Action = SWITCH(TRUE(), Tickets[Priority] = "High" && Tickets[Hours] > 24, "Escalate", "Monitor")

// New measure (shown on a Card)
Late % = DIVIDE(COUNTROWS(FILTER(Tickets, Tickets[Speed Band] = "Late")), COUNTROWS(Tickets))
```
</details>

<details>
<summary>Video 4: ALL vs ALLSELECTED</summary>

```dax
// New measures
Total Visits = SUM(Traffic[Visits])
Visits All Channels = CALCULATE([Total Visits], ALL(Traffic[Channel]))
% of Total = DIVIDE([Total Visits], [Visits All Channels])
Visits Selected = CALCULATE([Total Visits], ALLSELECTED(Traffic[Channel]))
% of Selected = DIVIDE([Total Visits], [Visits Selected])
Running Total = CALCULATE([Total Visits], FILTER(ALLSELECTED(Traffic[Month Start]), Traffic[Month Start] <= MAX(Traffic[Month Start])))
```
</details>

<details>
<summary>Video 5: RANKX, TOPN and SELECTEDVALUE</summary>

```dax
// New measures
Total Deals = SUM(Reps[Deals])
Rank = RANKX(ALL(Reps[Rep]), [Total Deals])
Rank Dense = RANKX(ALL(Reps[Rep]), [Total Deals], , DESC, Dense)
Top 3 Deals = CALCULATE([Total Deals], TOPN(3, ALL(Reps[Rep]), [Total Deals]))
Top 3 Deals (strict) = CALCULATE([Total Deals], TOPN(3, ALL(Reps[Rep]), [Total Deals], DESC, Reps[Rep], ASC))
Chart Title = "Deals closed: " & SELECTEDVALUE(Reps[Region], "All regions")
```
</details>

## Requirements

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows). Menu names may differ slightly between monthly releases.
- No other software or sign-in is needed. All data is small, fictional sample data created for teaching.

## Feedback and contributions

Found a typo or a formula that does not match the video? Please open an [issue](../../issues). Suggestions for future DAX topics are welcome in the comments on YouTube.

## Support the channel

If these tutorials helped you, please like the videos, share them with a colleague learning Power BI, and subscribe for new tutorials.

## License

This project is licensed under the [MIT License](LICENSE). You are free to use, copy, modify and share the formulas, sample data and reports, including for your own tutorials or training, as long as you keep the copyright notice. The sample data is fictional and created for teaching.
