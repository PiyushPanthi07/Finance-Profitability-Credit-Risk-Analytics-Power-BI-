## 2.KPI DAX Measures

This document contains the DAX measures used in the Power BI banking dashboard, organized by business functionality.

---

## 1. Loan Book Measures

### Last Available DateKey
Finds the latest available transaction date in the fact table up to the selected reporting date.

```dax
Last Available DateKey =
VAR SelectedEndDate =
    MAX('Dim_Date'[Date])
RETURN
    CALCULATE(
        MAX('Fact_Banking_Monthly'[DateKey]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey]
            <= VALUE(FORMAT(SelectedEndDate, "YYYYMMDD"))
    )
```

### Loan Book
Calculates the total closing principal for the latest available reporting date.

```dax
Loan Book =
VAR SelectedEndDate =
    MAX('Dim_Date'[Date])
VAR LastAvailableDateKey =
    CALCULATE(
        MAX('Fact_Banking_Monthly'[DateKey]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey]
            <= VALUE(FORMAT(SelectedEndDate, "YYYYMMDD"))
    )
RETURN
    CALCULATE(
        SUM('Fact_Banking_Monthly'[ClosingPrincipal]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] = LastAvailableDateKey
    )
```

### Loan Book Benchmark
Retrieves the loan book value for the previous month's ending date.

```dax
Loan Book Benchmark =
VAR CurrentEndDate =
    MAX('Dim_Date'[Date])
VAR PreviousDate =
    EOMONTH(CurrentEndDate, -1)
RETURN
    CALCULATE(
        [Loan Book],
        'Dim_Date'[Date] = PreviousDate
    )
```

### Loan Book Benchmark Display
Displays the benchmark value alongside the percentage change and directional indicator.

```dax
Loan Book Benchmark Display =
VAR BenchmarkValue = [Loan Book Benchmark]
VAR CurrentValue = [Loan Book]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow =
    IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
    "Benchmark: ₹"
        & FORMAT(BenchmarkValue / 1000000000, "0.0B")
        & " | "
        & Arrow
        & " "
        & FORMAT(ABS(VariancePct), "0.0%")
```

---

## 2. Financial Performance Benchmarks

### Interest Income Benchmark
Calculates the previous month's interest income.

```dax
Interest Income Benchmark =
CALCULATE(
    [Interest Income],
    DATEADD('Dim_Date'[Date], -1, MONTH)
)
```

### Interest Income Benchmark Display
Displays the benchmark interest income and percentage variance.

```dax
Interest Income Benchmark Display =
VAR BenchmarkValue = [Interest Income Benchmark]
VAR CurrentValue = [Interest Income]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow =
    IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
    "Benchmark: ₹"
        & FORMAT(BenchmarkValue / 1000000, "0.0M")
        & " | "
        & Arrow
        & " "
        & FORMAT(ABS(VariancePct), "0.0%")
```

### NII Benchmark
Calculates the previous month's Net Interest Income (NII).

```dax
NII Benchmark =
CALCULATE(
    [NII],
    DATEADD('Dim_Date'[Date], -1, MONTH)
)
```

### NII Benchmark Display
Displays the benchmark NII and percentage variance.

```dax
NII Benchmark Display =
VAR BenchmarkValue = [NII Benchmark]
VAR CurrentValue = [NII]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow =
    IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
    "Benchmark: ₹"
        & FORMAT(BenchmarkValue / 1000000, "0.0M")
        & " | "
        & Arrow
        & " "
        & FORMAT(ABS(VariancePct), "0.0%")
```

### Net Profit Benchmark
Calculates the previous month's net profit.

```dax
Net Profit Benchmark =
CALCULATE(
    [Net Profit],
    DATEADD('Dim_Date'[Date], -1, MONTH)
)
```

### Net Profit Benchmark Display
Displays the benchmark net profit and percentage variance.

```dax
Net Profit Benchmark Display =
VAR BenchmarkValue = [Net Profit Benchmark]
VAR CurrentValue = [Net Profit]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow =
    IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
    "Benchmark: ₹"
        & FORMAT(BenchmarkValue / 1000000, "0.0M")
        & " | "
        & Arrow
        & " "
        & FORMAT(ABS(VariancePct), "0.0%")
```

---

## 3. Banking Profitability & Efficiency Ratios

### Net Interest Margin (NIM)
Measures the annualized net interest income relative to average earning assets.

```dax
Net Interest Margin =
DIVIDE(
    [NII],
    SUM('Fact_Banking_Monthly'[AverageEarningAssets])
) * 12
```

### Cost of Funds
Measures the annualized interest expense relative to the average funding balance.

```dax
Cost of Funds =
DIVIDE(
    [Interest Expense],
    SUM('Fact_Banking_Monthly'[AverageFundingBalance])
) * 12
```

### Yield on Advances
Measures the annualized interest income relative to average earning assets.

```dax
Yield on Advances =
DIVIDE(
    [Interest Income],
    SUM('Fact_Banking_Monthly'[AverageEarningAssets])
) * 12
```

### Net Profit Margin
Measures net profit as a proportion of operating income.

```dax
Net Profit Margin =
DIVIDE(
    [Net Profit],
    [Operating Income]
)
```

---

## 4. Measure Summary

| Category | Measures |
|---|---|
| Loan Book | Last Available DateKey, Loan Book |
| Loan Book Benchmark | Loan Book Benchmark, Loan Book Benchmark Display |
| Financial Benchmarks | Interest Income Benchmark, NII Benchmark, Net Profit Benchmark |
| Benchmark Displays | Interest Income Benchmark Display, NII Benchmark Display, Net Profit Benchmark Display |
| Profitability Ratios | Net Interest Margin, Cost of Funds, Yield on Advances, Net Profit Margin |

---

**Note:** All benchmark display measures use directional arrows to indicate positive or negative percentage variance. Monetary values are displayed in millions (M) or billions (B) as appropriate.
