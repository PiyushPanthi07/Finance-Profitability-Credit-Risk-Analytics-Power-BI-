# Meridian Corporate Bank — Profitability & Credit Risk Analytics (Power BI)

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-44_Measures_%7C_Calc_Groups-0078D4?style=for-the-badge)
![Data Model](https://img.shields.io/badge/Model-Kimball_Star_Schema-15803D?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Corporate_Banking_%26_Risk-581C87?style=for-the-badge)

An executive 3-page Power BI decision-support dashboard analysing a **₹20.74 Billion** corporate lending portfolio across **120 corporate clients**, **240 credit facilities**, **4 loan products**, and **8 Indian corporate branches** (FY 2025). Built on a Star Schema with **44 modular DAX measures**, **semi-additive snapshot logic**, and a reusable **Calculation Group** (`Normal`, `MTD`, `QTD`, `R3M`).

> **[Read Executive Findings & Recommendations →](./INSIGHTS.md)** | **[Explore Full DAX Library →](./dax/README.md)** | **[View Slide Deck (PDF) →](./presentation/Meridian_Corporate_Bank_PowerBI_Project.pdf)**

---

## Dashboard Showcase

### Page 1 — Executive Profitability & Risk Overview

Tracks top-to-bottom P&L performance, lending spread gauges (NIM, Cost of Funds, Yield on Advances), a 7-stage Profitability Bridge waterfall, and portfolio risk exposure boxes with dynamic time-intelligence slicers.

![Page 1 — Profitability Overview](./assets/Finance%20Dashboard-%201.Profitability.PNG)

### Page 2 — Facility-Level Performance & Risk

Facility-by-facility operational matrix (`MCB0011`–`MCB1202`) highlighting Loan Book, Net Profit trends, Provision charges, colour-coded Risk Grades, Days Past Due tiers, and inline data bars for Overdue Principal and Facility Limits.

![Page 2 — Facility Details](./assets/Finance%20Dashboard-%202.Facility%20Details.PNG)

### Page 3 — Client Portfolio Summary

Industry-level drill-down (`Industry` → `ClientName`) surfacing High-Risk Exposure %, NIM compression, overdue balances, and provisioning intensity across every corporate borrower.

![Page 3 — Client Details](./assets/Finance%20Dashboard-%203.Client%20Details.PNG)

---

## FY 2025 Portfolio Snapshot

| Category | Key Metric | FY 2025 / Dec 2025 Value | Context |
| :--- | :--- | :---: | :--- |
| **Scale** | Loan Book (Dec Snapshot) | **₹20.74B** | +7.6% vs. Jan 2025 (₹19.28B) |
| **Revenue** | Interest Income | **₹1.95B** | Peak in Dec at ₹174.23M (+20.9% vs. Feb) |
| **Core Spread** | Net Interest Income (NII) | **₹993.34M** | ▲ 10.3% vs. prior period benchmark |
| **Bottom Line** | Net Profit | **₹479.20M** | ▲ 11.3% vs. prior period benchmark (44.21% NPM) |
| **Margins** | NIM / CoF / YoA | **5.05% / 5.16% / 9.91%** | Annualised ratios across earning assets & funding |
| **Credit Health** | Performing Loan Book (`DPD = 0`) | **₹17.01B** | 82.01% of active lending book |
| **Credit Stress** | High-Risk & `30+ DPD` Exposure | **₹3.73B** | 17.99% of book (`90+ DPD`: ₹180.79M / 0.87%) |
| **Liquidity** | Available Undrawn Limit | **₹11.01B** | 34.68% of ₹31.75B total sanctioned ceiling |

---

## Data Model Architecture

Structured as a Kimball Star Schema (`1:*` single-direction filters) connecting `Fact_Banking_Monthly` (**2,880 monthly facility snapshots**) to 4 dimensions (`Dim_Date`, `Dim_Client`, `Dim_Product`, `Dim_Branch`), supported by disconnected analytical tables (`Profitability_Bridge`, `Time Period` Calculation Group, and `Z_Measures`).

<details>
<summary><strong>Click to view Data Model Diagram & Schema Summary</strong></summary>

<br>

![Data Model Schema](./docs/Data%20Model.png)

- **`Fact_Banking_Monthly`** (2,880 rows × 23 cols): Monthly grain (`DateKey`, `FacilityID`) tracking opening/closing balances, disbursements, repayments, P&L flows, overdue principal, DPD, and risk grades.
- **`Dim_Date`** (365 rows): DAX calendar table (`2025-01-01` to `2025-12-31`), marked as official Date Table with sort keys.
- **`Dim_Client`** (120 rows): Corporate entities across 6 industries and 3 segments (`Large`, `Mid`, `Emerging Corporate`).
- **`Dim_Product`** (4 rows): `Corporate Term Loan`, `Trade Finance Loan`, `Working Capital Revolver`, `Equipment Finance Loan`.
- **`Dim_Branch`** (8 rows): `Mumbai`, `Pune`, `Delhi`, `Chandigarh`, `Bengaluru`, `Chennai`, `Kolkata`, `Bhubaneswar`.

Full column-level definitions and visual layout specs are documented in [`docs/data_dictionary_and_visual_specs.md`](./docs/data_dictionary_and_visual_specs.md).

</details>

---

## DAX Architecture & Formula Drill-Down

To keep the repository clean and easy to audit, all **3 Calculated Tables**, **1 Calculation Group**, and **44 DAX Measures** (organised into `Z_Measures` display folders inside the `.pbix`) are modularised by category inside [`/dax`](./dax/README.md). Click any module below to inspect the exact DAX code:

| Module | Power BI Location | Contents | Formulas |
| :--- | :--- | :--- | :---: |
| **[Calculated Tables & Calc Group](./dax/00_calculated_tables_and_calculated_groups.md)** | Model Tables & `Time Period` | `Dim_Date`, `Profitability_Bridge`, `Z_Measures`, and `Normal` / `MTD` / `QTD` / `R3M` items | 3 Tables + 4 Items |
| **[01. Base Measures](./dax/01_base_measures.md)** | `Z_Measures / Base Measures` | Core flow P&L aggregations (`Interest Income`, `NII`, `Operating Income`, `Net Profit`, etc.) | 9 |
| **[02. KPI & Margin Measures](./dax/02_kpi_measures.md)** | `Z_Measures / KPI` | Semi-additive `Loan Book`, `Last Available DateKey`, MoM benchmarks, `NIM`, `CoF`, `YoA`, `NPM` | 14 |
| **[03. Color Measures](./dax/03_color_measures.md)** | `Z_Measures / Color Measures` | Dynamic hex formatting measures (`#15803D` / `#B91C1C`) driven by benchmark variance | 4 |
| **[04. Waterfall & Risk Exposure](./dax/04_waterfall_risk_exposure.md)** | `Z_Measures / Waterfall & Risk` | `SWITCH()` P&L waterfall driver, `Performing Loan Book`, `30+ / 90+ DPD`, `Available Limit` | 10 |
| **[05. Matrix Snapshot Measures](./dax/05_matrix_measures.md)** | `Z_Measures / Matrix Measures` | Point-in-time facility & client matrix metrics (`Current Risk Grade`, `Current DPD`, `High-Risk %`) | 7 |

---

## What I Learned

- **Semi-Additive Balance Sheet vs. Flow Engineering:** Separating additive P&L flows (`Interest Income`, `Net Profit`) from point-in-time balance sheet snapshots (`Loan Book`, `Facility Limit`, `High-Risk Exposure`) so multi-month selections never double-count principal balances.
- **Mid-Month Non-Blank Date Resolution:** Building a `Last Available DateKey` pattern with `REMOVEFILTERS('Dim_Date')` so selecting any mid-month date on a daily calendar slicer cleanly resolves to the latest month-end snapshot instead of returning blanks.
- **Scaling Time Intelligence with Calculation Groups:** Replacing dozens of redundant `MTD` / `QTD` / `R3M` measures with a single `Time Period` Calculation Group using `SELECTEDMEASURE()` and `EOMONTH()` rolling-window logic.
- **Custom P&L Waterfall Mechanics:** Pairing a disconnected `DATATABLE` (`Profitability_Bridge`) with `SWITCH(SELECTEDVALUE(...))` to walk executives visually from gross Interest Income (₹1.95B) through funding, operating, credit, and tax deductions down to Net Profit (₹479.20M).
- **Corporate Banking Domain Fluency:** Translating lending spreads (`Yield on Advances` vs. `Cost of Funds` → `NIM`), Expected Credit Loss (ECL) provisioning shocks, and DPD ageing buckets (`30+ DPD` vs. `90+ DPD`) into an executive-ready analytical layout.
- **Maintainable Semantic Model Hygiene:** Grouping 44 measures inside a dedicated `Z_Measures` display-folder hierarchy and driving conditional formatting through decoupled hex-colour measures for consistent visual governance.

---

## Repository Navigation & Quick Start

1. Download [`powerbi/Meridian_Corporate_Bank_Analytics.pbix`](./powerbi/Meridian_Corporate_Bank_Analytics.pbix) and [`data/Meridian_Corporate_Bank_2025.xlsx`](./data/Meridian_Corporate_Bank_2025.xlsx).
2. Open the `.pbix` file in **Power BI Desktop** (`Transform Data` → `Data Source Settings` → update the local path to `Meridian_Corporate_Bank_2025.xlsx` if refreshing).
3. Review the executive analysis in [`INSIGHTS.md`](./INSIGHTS.md) or browse the [`dax/`](./dax/README.md) directory for reusable banking DAX patterns.

---

## Author

**Piyush Panthi**  
[![GitHub](https://img.shields.io/badge/GitHub-PiyushPanthi07-181717?style=flat&logo=github)]([https://github.com/PiyushPanthi07](https://github.com/PiyushPanthi07))
[![LinkedIn](https://img.shields.io/badge/LinkedIn-piyushpanthi-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/piyushpanthi)
