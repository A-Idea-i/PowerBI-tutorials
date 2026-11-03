# Power BI DAX Spotlight

Five short, standalone Power BI tutorials, each focused on one DAX idea. Every video has its own tiny sample dataset, so you can watch them in any order.

> **Playlist:** [Power BI DAX Spotlight on YouTube](YOUR-PLAYLIST-LINK)

## What you'll learn

| # | Video | DAX covered | Data file |
|---|-------|-------------|-----------|
| 1 | [CALENDARAUTO(): build a date table in one line](YOUR-VIDEO-1-LINK) | `CALENDARAUTO`, `YEAR`, `MONTH`, `FORMAT`, `ROUNDUP` | `data/Events.csv` |
| 2 | [Time intelligence: YTD, last year and growth %](YOUR-VIDEO-2-LINK) | `TOTALYTD`, `SAMEPERIODLASTYEAR`, `DATEADD`, `DIVIDE`, `CALCULATE` | `data/Subscriptions.csv` |
| 3 | [SWITCH(TRUE()): say goodbye to nested IFs](YOUR-VIDEO-3-LINK) | `SWITCH`, `TRUE`, `IF`, `FILTER`, `COUNTROWS` | `data/Tickets.csv` |
| 4 | [ALL vs ALLSELECTED: percent of total and running total](YOUR-VIDEO-4-LINK) | `ALL`, `ALLSELECTED`, `CALCULATE`, `FILTER`, `MAX` | `data/Traffic.csv` |
| 5 | [RANKX, TOPN and dynamic titles](YOUR-VIDEO-5-LINK) | `RANKX`, `TOPN`, `SELECTEDVALUE`, `ALL` | `data/Reps.csv` |

## Repository structure

```
dax-spotlight/
├── README.md
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
5. Type the formulas from the video using **New measure**, **New column** or **New table**.

**Tip for Video 1:** turn off Auto date/time for the file under **File > Options and settings > Options > Current File > Data Load**. To turn it off for every new report, use **Global > Data Load** and untick **Auto date/time for new files**.

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

- Speed bands: **3 Fast, 4 On Time, 3 Late**.
- `Late %` = **30%**.
- Tickets flagged "Escalate": **T007** and **T009**.

### Video 4: Traffic

| Column | Type |
|--------|------|
| Month Start | Date |
| Channel | Text |
| Visits | Whole number |

| Channel | Visits | % of Total |
|---------|--------|------------|
| Search | 1,650 | 47.1% |
| Social | 900 | 25.7% |
| Email | 370 | 10.6% |
| Direct | 580 | 16.6% |

- With only **Search** and **Social** selected, `% of Selected` is **64.7%** and **35.3%**.
- Running total: January 1,100, February 2,300, March 3,500.

### Video 5: Reps

| Column | Type |
|--------|------|
| Rep | Text |
| Region | Text |
| Deals | Whole number |

- Total deals: **124**.
- `Rank` gives Dev and Hiro a tie at 3; `Rank Dense` makes Ana 4, Grace 5, and Ben and Elena 6.
- `TOPN(3)` returns four reps because of the tie, so `Top 3 Deals` = **80**.
- Selecting **East** gives the title "Deals closed: East" and 46 deals.

## Formulas used

<details>
<summary>Video 1: CALENDARAUTO()</summary>

```dax
Calendar = CALENDARAUTO()
Calendar FY = CALENDARAUTO(3)
Year = YEAR('Calendar'[Date])
Month Number = MONTH('Calendar'[Date])
Month Name = FORMAT('Calendar'[Date], "MMM")
Quarter = "Q" & ROUNDUP(MONTH('Calendar'[Date]) / 3, 0)
```
</details>

<details>
<summary>Video 2: Time intelligence</summary>

```dax
Total Revenue = SUM(Subscriptions[Revenue])
Revenue YTD = TOTALYTD([Total Revenue], 'Calendar'[Date])
Revenue Last Year = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Calendar'[Date]))
YoY % = DIVIDE([Total Revenue] - [Revenue Last Year], [Revenue Last Year])
```
</details>

<details>
<summary>Video 3: SWITCH(TRUE())</summary>

```dax
Speed Band (IF) = IF(Tickets[Hours] <= 4, "Fast", IF(Tickets[Hours] <= 24, "On Time", "Late"))
Speed Band = SWITCH(TRUE(), Tickets[Hours] <= 4, "Fast", Tickets[Hours] <= 24, "On Time", "Late")
Late % = DIVIDE(COUNTROWS(FILTER(Tickets, Tickets[Speed Band] = "Late")), COUNTROWS(Tickets))
Action = SWITCH(TRUE(), Tickets[Priority] = "High" && Tickets[Hours] > 24, "Escalate", "Monitor")
```
</details>

<details>
<summary>Video 4: ALL vs ALLSELECTED</summary>

```dax
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
Total Deals = SUM(Reps[Deals])
Rank = RANKX(ALL(Reps[Rep]), [Total Deals])
Rank Dense = RANKX(ALL(Reps[Rep]), [Total Deals], , DESC, Dense)
Top 3 Deals = CALCULATE([Total Deals], TOPN(3, ALL(Reps[Rep]), [Total Deals]))
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

Sample data and formulas are provided for educational use. Add your preferred license here (for example MIT) before publishing.
