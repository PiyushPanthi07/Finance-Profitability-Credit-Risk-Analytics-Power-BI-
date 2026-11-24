## 4. Conditional Formatting Measures

These measures dynamically assign colors to financial KPI visuals based on their percentage variance against the respective benchmarks.

### Loan Book Color
Returns green when the loan book meets or exceeds the benchmark; otherwise, returns red.

```dax
Loan Book Color =
VAR BenchmarkValue = [Loan Book Benchmark]
VAR CurrentValue = [Loan Book]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
    IF(
        VariancePct >= 0,
        "#15803D",
        "#B91C1C"
    )
```

### Interest Income Color
Returns green when interest income meets or exceeds the benchmark; otherwise, returns red.

```dax
Interest Income Color =
VAR BenchmarkValue = [Interest Income Benchmark]
VAR CurrentValue = [Interest Income]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
    IF(
        VariancePct >= 0,
        "#15803D",
        "#B91C1C"
    )
```

### NII Color
Returns green when Net Interest Income meets or exceeds the benchmark; otherwise, returns red.

```dax
NII Color =
VAR BenchmarkValue = [NII Benchmark]
VAR CurrentValue = [NII]
VAR VariancePct =
    DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
    IF(
        VariancePct >= 0,
        "#15803D",
        "#B91C1C"
    )
```

### Net Profit Color
Returns green when net profit meets or exceeds the benchmark; otherwise, returns red.

```dax
Net Profit Color =
VAR
