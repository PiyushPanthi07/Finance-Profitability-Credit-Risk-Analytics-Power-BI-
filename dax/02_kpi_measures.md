Last Available DateKey = 
VAR SelectedEndDate = MAX('Dim_Date'[Date])
RETURN
CALCULATE(
    MAX('Fact_Banking_Monthly'[DateKey]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] <= VALUE(FORMAT(SelectedEndDate, "YYYYMMDD"))
)

Loan Book = 
VAR SelectedEndDate = MAX('Dim_Date'[Date])
VAR LastAvailableDateKey = 
    CALCULATE(
        MAX('Fact_Banking_Monthly'[DateKey]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] <= VALUE(FORMAT(SelectedEndDate, "YYYYMMDD"))
    )
RETURN
CALCULATE(
    SUM('Fact_Banking_Monthly'[ClosingPrincipal]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] = LastAvailableDateKey
)

Loan Book Benchmark = 
VAR CurrentEndDate = MAX('Dim_Date'[Date])
VAR PreviousDate = EOMONTH(CurrentEndDate, -1)
RETURN
CALCULATE(
    [Loan Book],
    'Dim_Date'[Date] = PreviousDate
)

Loan Book Benchmark Display = 
VAR BenchmarkValue = [Loan Book Benchmark]
VAR CurrentValue = [Loan Book]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow = IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
"Benchmark: ₹" & FORMAT(BenchmarkValue / 1000000000, "0.0B") & " | " & Arrow & " " & FORMAT(ABS(VariancePct), "0.0%")

Interest Income Benchmark = 
CALCULATE(
    [Interest Income],
    DATEADD('Dim_Date'[Date], -1, MONTH)
)

Interest Income Benchmark Display = 
VAR BenchmarkValue = [Interest Income Benchmark]
VAR CurrentValue = [Interest Income]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow = IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
"Benchmark: ₹" & FORMAT(BenchmarkValue / 1000000, "0.0M") & " | " & Arrow & " " & FORMAT(ABS(VariancePct), "0.0%")

NII Benchmark = 
CALCULATE(
    [NII],
    DATEADD('Dim_Date'[Date], -1, MONTH)
)

NII Benchmark Display = 
VAR BenchmarkValue = [NII Benchmark]
VAR CurrentValue = [NII]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow = IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
"Benchmark: ₹" & FORMAT(BenchmarkValue / 1000000, "0.0M") & " | " & Arrow & " " & FORMAT(ABS(VariancePct), "0.0%")

Net Profit Benchmark = 
CALCULATE(
    [Net Profit],
    DATEADD('Dim_Date'[Date], -1, MONTH)
)

Net Profit Benchmark Display = 
VAR BenchmarkValue = [Net Profit Benchmark]
VAR CurrentValue = [Net Profit]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
VAR Arrow = IF(VariancePct >= 0, UNICHAR(9650), UNICHAR(9660))
RETURN
"Benchmark: ₹" & FORMAT(BenchmarkValue / 1000000, "0.0M") & " | " & Arrow & " " & FORMAT(ABS(VariancePct), "0.0%")

Net Interest Margin = 
DIVIDE([NII], SUM('Fact_Banking_Monthly'[AverageEarningAssets])) * 12

Cost of Funds = 
DIVIDE([Interest Expense], SUM('Fact_Banking_Monthly'[AverageFundingBalance])) * 12

Yield on Advances = 
DIVIDE([Interest Income], SUM('Fact_Banking_Monthly'[AverageEarningAssets])) * 12

Net Profit Margin = 
DIVIDE([Net Profit], [Operating Income])

