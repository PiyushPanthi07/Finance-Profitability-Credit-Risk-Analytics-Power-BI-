Loan Book Color = 
VAR BenchmarkValue = [Loan Book Benchmark]
VAR CurrentValue = [Loan Book]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
IF(VariancePct >= 0, "#15803D", "#B91C1C")

Interest Income Color = 
VAR BenchmarkValue = [Interest Income Benchmark]
VAR CurrentValue = [Interest Income]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
IF(VariancePct >= 0, "#15803D", "#B91C1C")

NII Color = 
VAR BenchmarkValue = [NII Benchmark]
VAR CurrentValue = [NII]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
IF(VariancePct >= 0, "#15803D", "#B91C1C")

Net Profit Color = 
VAR BenchmarkValue = [Net Profit Benchmark]
VAR CurrentValue = [Net Profit]
VAR VariancePct = DIVIDE(CurrentValue - BenchmarkValue, BenchmarkValue)
RETURN
IF(VariancePct >= 0, "#15803D", "#B91C1C")
