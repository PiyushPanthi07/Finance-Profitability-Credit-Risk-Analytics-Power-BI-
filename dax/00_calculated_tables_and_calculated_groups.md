# DAX Calculated Tables & Calculation Groups

This document contains the calculated tables and calculation group used in the Corporate Banking Power BI dashboard.

---

# 1. Calculated Tables

## 1.1 Dim_Date — Calendar Dimension

Creates an official calendar dimension covering 01 January 2025 to 31 December 2025, with additional date attributes for time-based analysis.

**Configuration:**
- Mark as Date Table using `[Date]`.
- Sort `[Date]` by `[DateKey]`.
- Sort `[Quarter]` by `[QuarterNumber]`.
- Sort `[Month]` by `[MonthNumber]`.

```dax
Dim_Date =
ADDCOLUMNS(
    CALENDAR(
        DATE(2025, 1, 1),
        DATE(2025, 12, 31)
    ),
    "DateKey", VALUE(FORMAT([Date], "YYYYMMDD")),
    "Year", YEAR([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "QuarterNumber", QUARTER([Date]),
    "Month", FORMAT([Date], "MMM"),
    "MonthNumber", MONTH([Date])
)
```

**Generated Columns:**

| Column | Data Type | Description |
|---|---|---|
| Date | Date | Calendar date |
| DateKey | Whole Number | Date in YYYYMMDD format |
| Year | Whole Number | Calendar year |
| Quarter | Text | Quarter label (Q1–Q4) |
| QuarterNumber | Whole Number | Quarter number (1–4) |
| Month | Text | Abbreviated month name |
| MonthNumber | Whole Number | Month number (1–12) |

<text color="secondary" size="xs">Expected row count: 365</text>

---

## 1.2 Profitability_Bridge — Waterfall Helper Table

Creates a disconnected helper table containing the seven financial stages used in
