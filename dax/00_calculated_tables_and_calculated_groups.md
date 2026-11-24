// ============================================================================
// CORPORATE BANK — CALCULATED TABLES
// ============================================================================

// 1. Official Calendar Dimension Table (365 Rows: 01-Jan-2025 to 31-Dec-2025)
// Configuration:
// - Mark as Date Table on [Date]
// - Sort [Date] by [DateKey]
// - Sort [Quarter] by [QuarterNumber]
// - Sort [Month] by [MonthNumber]
Dim_Date = 
ADDCOLUMNS(
    CALENDAR(DATE(2025, 1, 1), DATE(2025, 12, 31)),
    "DateKey", VALUE(FORMAT([Date], "YYYYMMDD")),
    "Year", YEAR([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "QuarterNumber", QUARTER([Date]),
    "Month", FORMAT([Date], "MMM"),
    "MonthNumber", MONTH([Date])
)

// 2. Disconnected Helper Table for Profitability Bridge Waterfall Chart (7 Rows)
// Configuration:
// - Sort [Stage] by [SortOrder]
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

// 3. Dedicated Container Table for DAX Measures
Z_Measures = { BLANK() }


// ============================================================================
// CORPORATE BANK — CALCULATION GROUP: 'Time Period'
// Column Name: [Time Period]
// Used on the 'Scenario' 2x2 Slicer across all 3 pages
// ============================================================================

// Calculation Item 1: Normal (Default active period)
Normal = 
SELECTEDMEASURE()

// Calculation Item 2: Month-to-Date (MTD)
MTD = 
CALCULATE(
    SELECTEDMEASURE(),
    DATESMTD('Dim_Date'[Date])
)

// Calculation Item 3: Quarter-to-Date (QTD)
QTD = 
CALCULATE(
    SELECTEDMEASURE(),
    DATESQTD('Dim_Date'[Date])
)

// Calculation Item 4: Rolling 3 Months (R3M)
R3M = 
VAR EndDate = MAX('Dim_Date'[Date])
VAR StartDate = EOMONTH(EndDate, -3) + 1
RETURN
CALCULATE(
    SELECTEDMEASURE(),
    DATESBETWEEN('Dim_Date'[Date], StartDate, EndDate)
)