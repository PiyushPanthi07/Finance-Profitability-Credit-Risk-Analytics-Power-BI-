Interest Income = SUM('Fact_Banking_Monthly'[InterestIncome])

Interest Expense = SUM('Fact_Banking_Monthly'[InterestExpense])

Fee Income = SUM('Fact_Banking_Monthly'[FeeIncome])

Operating Expense = SUM('Fact_Banking_Monthly'[OperatingExpense])

Provision Expense = SUM('Fact_Banking_Monthly'[ProvisionExpense])

Tax Expense = SUM('Fact_Banking_Monthly'[TaxExpense])

NII = [Interest Income] - [Interest Expense]

Operating Income = [NII] + [Fee Income]

Net Profit = 
[NII] + [Fee Income] - [Operating Expense] - [Provision Expense] - [Tax Expense]
