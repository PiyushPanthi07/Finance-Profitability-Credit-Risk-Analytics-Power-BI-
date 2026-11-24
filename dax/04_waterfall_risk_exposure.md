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

Performing Loan Book = 
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[DaysPastDue] = 0
)

Overdue Principal = SUM('Fact_Banking_Monthly'[OverduePrincipal])

High-Risk Exposure = 
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[RiskGrade] = "High"
)

Available Limit = 
VAR CurrentLimit = 
    CALCULATE(
        SUM('Fact_Banking_Monthly'[FacilityLimit]),
        LASTDATE('Dim_Date'[Date])
    )
RETURN
CurrentLimit - [Loan Book]

Total Risk Exposure = 
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[RiskGrade] IN {"Medium", "High"}
)

Provision Amount = SUM('Fact_Banking_Monthly'[ProvisionExpense])

30+ DPD Exposure = 
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[DaysPastDue] >= 30
)

90+ DPD Exposure = 
CALCULATE(
    [Loan Book],
    'Fact_Banking_Monthly'[DaysPastDue] >= 90
)

Average DPD = AVERAGE('Fact_Banking_Monthly'[DaysPastDue])
