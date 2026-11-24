Loan Book 1 = 
VAR LastDateKey = [Last Available DateKey]
RETURN
CALCULATE(
    SUM('Fact_Banking_Monthly'[ClosingPrincipal]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] = LastDateKey
)

Current Risk Grade = 
VAR LastDateKey = [Last Available DateKey]
RETURN
CALCULATE(
    SELECTEDVALUE('Fact_Banking_Monthly'[RiskGrade]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] = LastDateKey
)

Current Days Past Due = 
VAR LastDateKey = [Last Available DateKey]
RETURN
CALCULATE(
    MAX('Fact_Banking_Monthly'[DaysPastDue]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] = LastDateKey
)

Current Overdue Principal = 
VAR LastDateKey = [Last Available DateKey]
RETURN
CALCULATE(
    SUM('Fact_Banking_Monthly'[OverduePrincipal]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] = LastDateKey
)

Current Facility Limit = 
VAR LastDateKey = [Last Available DateKey]
RETURN
CALCULATE(
    SUM('Fact_Banking_Monthly'[FacilityLimit]),
    REMOVEFILTERS('Dim_Date'),
    'Fact_Banking_Monthly'[DateKey] = LastDateKey
)

High-Risk Exposure % = 
COALESCE(
    DIVIDE([High-Risk Exposure], [Loan Book]),
    0
)

Selected Client = 
SELECTEDVALUE('Dim_Client'[ClientName], "Client Detail")