## 4. Profitability Bridge Measures

### Profitability Bridge Value
Returns the corresponding financial value for each stage of the profitability waterfall. Expenses are converted to negative values to represent deductions.

```dax
Profitability Bridge Value =
SWITCH(
    SELECTEDVALUE('Profitability_Bridge'[Stage]),
    "Interest Income", [Interest Income],
    "Interest Expense", -[Interest Expense],
    "Fee Income", [Fee Income],
    "Operating Expense", -[Operating Expense],
    "Provision Expense", -[Provision Expense],
    "Tax Expense", -[Tax Expense],
    "Net Profit", [Net Profit]
)
```

---

## 6. Loan Portfolio Measures

### Performing Loan Book
Calculates the outstanding loan book for loans with zero days past due (DPD).

```dax
Performing Loan Book =
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[DaysPastDue] = 0
)
```

### Overdue Principal
Calculates the total overdue principal amount.

```dax
Overdue Principal =
SUM('Fact_Banking_Monthly'[OverduePrincipal])
```

### Available Limit
Calculates the unused facility limit by subtracting the loan book from the total facility limit.

```dax
Available Limit =
VAR CurrentLimit =
    CALCULATE(
        SUM('Fact_Banking_Monthly'[FacilityLimit]),
        LASTDATE('Dim_Date'[Date])
    )
RETURN
    CurrentLimit - [Loan Book]
```

---

## 7. Credit Risk Exposure Measures

### High-Risk Exposure
Calculates the outstanding loan book classified under the High risk grade.

```dax
High-Risk Exposure =
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[RiskGrade] = "High"
)
```

### Total Risk Exposure
Calculates the outstanding loan book classified under Medium and High risk grades.

```dax
Total Risk Exposure =
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[RiskGrade] IN {"Medium", "High"}
)
```

### 30+ DPD Exposure
Calculates the outstanding loan book for accounts overdue by 30 days or more.

```dax
30+ DPD Exposure =
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[DaysPastDue] >= 30
)
```

### 90+ DPD Exposure
Calculates the outstanding loan book for accounts overdue by 90 days or more.

```dax
90+ DPD Exposure =
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[DaysPastDue] >= 90
)
```

---

## 8. Credit Risk & Provision Measures

### Provision Amount
Calculates the total provision expense recorded in the fact table.

```dax
Provision Amount =
SUM('Fact_Banking_Monthly'[ProvisionExpense])
```

### Average DPD
Calculates the average number of days past due across the filtered loan records.

```dax
Average DPD =
AVERAGE('Fact_Banking_Monthly'[DaysPastDue])
```

---

## 9. Measure Summary

| Category | Measures |
|---|---|
| Profitability Bridge | Profitability Bridge Value |
| Loan Portfolio | Performing Loan Book, Overdue Principal, Available Limit |
| Credit Risk Exposure | High-Risk Exposure, Total Risk Exposure |
| Delinquency | 30+ DPD Exposure, 90+ DPD Exposure, Average DPD |
| Provisions | Provision Amount |
