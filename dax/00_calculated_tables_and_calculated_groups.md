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

Creates a disconnected helper table containing the seven financial stages used in the profitability bridge waterfall chart.

**Configuration:**
- Sort `[Stage]` by `[SortOrder]`.
- Keep this table disconnected from the data model.
- Use `[Stage]` as the waterfall category.

```dax
Profitability_Bridge =
DATATABLE(
    "Stage", STRING,
    "SortOrder", INTEGER,
    {
        {"Interest Income", 1},
        {"Interest Expense", 2},
        {"Fee Income", 3},
        {"Operating Expense", 4},
        {"Provision Expense", 5},
        {"Tax Expense", 6},
        {"Net Profit", 7}
    }
)
```

**Generated Columns:**

| Column | Data Type | Description |
|---|---|---|
| Stage | Text | Financial stage displayed in the waterfall |
| SortOrder | Whole Number | Controls the display sequence |

<text color="secondary" size="xs">Expected row count: 7</text>

---

## 1.3 Z_Measures — Measure Container Table

Creates a dedicated container table for organizing DAX measures within the Power BI Fields pane.

```dax
Z_Measures =
{ BLANK() }
```

**Purpose:**
- Centralizes DAX measures.
- Keeps the Fields pane organized.
- Separates measures from business data tables.

---

# 2. Calculation Groups

## 2.1 Time Period — Calculation Group

**Calculation Group Name:** `Time Period`

**Column Name:** `[Time Period]`

**Purpose:** Provides dynamic time-period analysis across all three dashboard pages using a shared Scenario slicer.

### Calculation Item 1: Normal

Returns the selected measure without applying additional time intelligence calculations.

```dax
Normal =
SELECTEDMEASURE()
```

### Calculation Item 2: Month-to-Date (MTD)

Calculates the selected measure from the beginning of the current month through the selected date.

```dax
MTD =
CALCULATE(
    SELECTEDMEASURE(),
    DATESMTD('Dim_Date'[Date])
)
```

### Calculation Item 3: Quarter-to-Date (QTD)

Calculates the selected measure from the beginning of the current quarter through the selected date.

```dax
QTD =
CALCULATE(
    SELECTEDMEASURE(),
    DATESQTD('Dim_Date'[Date])
)
```

### Calculation Item 4: Rolling 3 Months (R3M)

Calculates the selected measure over a rolling three-calendar-month period ending on the selected date.

```dax
R3M =
VAR EndDate =
    MAX('Dim_Date'[Date])
VAR StartDate =
    EOMONTH(EndDate, -3) + 1
RETURN
    CALCULATE(
        SELECTEDMEASURE(),
        DATESBETWEEN(
            'Dim_Date'[Date],
            StartDate,
            EndDate
        )
    )
```

---

## 3. Calculation Group Summary

| Calculation Item | Abbreviation | Description |
|---|---|---|
| Normal | Normal | Default measure calculation |
| Month-to-Date | MTD | Current month cumulative calculation |
| Quarter-to-Date | QTD | Current quarter cumulative calculation |
| Rolling 3 Months | R3M | Rolling three-month calculation |

---

## 4. Model Configuration Summary

| Object | Type | Purpose |
|---|---|---|
| Dim_Date | Calculated Table | Calendar and time intelligence |
| Profitability_Bridge | Calculated Table | Waterfall chart stage ordering |
| Z_Measures | Calculated Table | DAX measure organization |
| Time Period | Calculation Group | Dynamic time-period analysis |
| Time Period[Time Period] | Calculation Group Column | Scenario slicer |

**Implementation Notes:**
- The calendar table covers the 2025 financial reporting year.
- The profitability bridge is intentionally disconnected.
- The Time Period calculation group uses `SELECTEDMEASURE()` to dynamically modify supported measures.
- The calculation group is intended to work with the shared Scenario slicer across all three report pages.
