## 5. Client-Level Loan Measures

### Loan Book 1
Calculates the closing principal for the latest available reporting date while preserving other filter contexts.

```dax
Loan Book 1 =
VAR LastDateKey = [Last Available DateKey]
RETURN
    CALCULATE(
        SUM('Fact_Banking_Monthly'[ClosingPrincipal]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] = LastDateKey
    )
```

### Current Risk Grade
Returns the risk classification of the selected client for the latest available reporting date.

```dax
Current Risk Grade =
VAR LastDateKey = [Last Available DateKey]
RETURN
    CALCULATE(
        SELECTEDVALUE('Fact_Banking_Monthly'[RiskGrade]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] = LastDateKey
    )
```

### Current Days Past Due
Returns the maximum days past due for the selected client at the latest available reporting date.

```dax
Current Days Past Due =
VAR LastDateKey = [Last Available DateKey]
RETURN
    CALCULATE(
        MAX('Fact_Banking_Monthly'[DaysPastDue]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] = LastDateKey
    )
```

### Current Overdue Principal
Calculates the overdue principal for the selected client at the latest available reporting date.

```dax
Current Overdue Principal =
VAR LastDateKey = [Last Available DateKey]
RETURN
    CALCULATE(
        SUM('Fact_Banking_Monthly'[OverduePrincipal]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] = LastDateKey
    )
```

### Current Facility Limit
Calculates the total facility limit for the selected client at the latest available reporting date.

```dax
Current Facility Limit =
VAR LastDateKey = [Last Available DateKey]
RETURN
    CALCULATE(
        SUM('Fact_Banking_Monthly'[FacilityLimit]),
        REMOVEFILTERS('Dim_Date'),
        'Fact_Banking_Monthly'[DateKey] = LastDateKey
    )
```

---

## 10. Risk Ratio Measures

### High-Risk Exposure %
Calculates the proportion of the loan book classified as high-risk. Returns zero when the calculation is blank or the denominator is zero.

```dax
High-Risk Exposure % =
COALESCE(
    DIVIDE([High-Risk Exposure], [Loan Book]),
    0
)
```

---

## 11. Dynamic Display Measures

### Selected Client
Returns the selected client's name. Displays "Client Detail" when no single client is selected.

```dax
Selected Client =
SELECTEDVALUE(
    'Dim_Client'[ClientName],
    "Client Detail"
)
```

---

## 12. Measure Summary

| Category | Measures |
|---|---|
| Client-Level Loan | Loan Book 1, Current Facility Limit |
| Client Risk Profile | Current Risk Grade, Current Days Past Due |
| Client Delinquency | Current Overdue Principal |
| Risk Ratios | High-Risk Exposure % |
| Dynamic Display | Selected Client |
