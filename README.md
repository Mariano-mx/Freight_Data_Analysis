# The Freight Recession: How U.S. Truckload Margins Turned Negative

**An analysis on demand, capacity, spot rates and operating costs, 2019–2024**

<!-- 1 sentence: what you did + tools + main result. This is the most-read line of the whole README. -->
End-to-end Excel analysis of U.S. freight indicators, built with **Power Query, Power Pivot (DAX) and an interactive dashboard**, showing how truckload carriers went from earning **+$0.91** per mile in 2021 to losing **−$0.19** per mile in 2024.

<img src="images/dashboard_interaction.gif" alt="Dashboard interaction" width="800">


<!-- Main GIF: 10–15 s using the slicers on the dashboard. -->

---

## Key Findings

<!-- 3–4 bullets, each one with a number. Verify every figure against your dashboard. -->
- **Demand boom (2021–2024):**
     - The years following the pandemic, **U.S. Goods Imports rose up to 22%**.
- **Capacity surge:**
     - In 2021, more than **80.1k new for-hire carriers** were added to the Motor Carrier Management Information System (MCMIS), a **71% increase** with respect to 2019. 
- **Rates fell, costs rose:**
     - Dry Van **spot rates fell 32%** (2021–2024) while non-fuel operating **costs rose 24%**.
- **Diesel was not a major cause of the downfall in spot rates:**
     - Dry van spot rates peaked in January 2022 ($2.68/mile) and had already fallen **28%** by the time diesel peaked in June 2022 ($5.75/gallon).

## The Question

<!-- 1 or 2 sentences. Add one line on why it matters to a carrier or a shipper — your transport experience helps here. -->
What drove U.S. truckload spot rates and carrier profitability between 2019 and 2024?

Land freight in the U.S. has been in a recession over the last 3 years, profitability has reduced significantly for the main vehicles (Dry Vans, Flatbeds and Refrigerated trailers) in comparison to previous years. I wanted to understand this phenomenon with the available public data.

The 2022-2024 freight recession was driven by **overcapacity**, not by weak demand or rises in fuel costs.
Demand has held up after 2020, rising up to 20% in 2022, and although there was a 2% pullback in 2023, it reached a 22% increase above pre-pandemic levels in 2024. However, **capacity grew even faster**: attracted by record rates, more than 80.1k for-hire carriers registered in the MCMIS in 2021, a 71% increase compared to 2019.

With more trucks competing for the same freight, dry van spot rates peaked in January 2022 and **fell 32% from 2021 to 2024**. Fuel was not the trigger: rates were already down 28% when diesel peaked in June 2022.

Meanwhile, non-fuel operating costs (wages, insurance, equipment) rose 24%. Squeezed from both sides, dry van margin per mile went from +$0.91 in 2021 to −$0.19 in 2024, so the spot market no longer covered the cost of running a truck. Refrigerated and flatbed margins stayed positive but shrank by more than 80%.

As demand remains strong, a recovery in rates depends on capacity leaving the market. The rise in new registrations in 2024 suggests that adjustment had not yet begun.

## Skills Used in this project

| Area | What I did |
|---|---|
| **Power Query (ETL)** | Cleaned 30K+ rows: text dates to real dates, broken encoding, blank values, weekly and monthly series grouped to monthly, annual costs expanded to monthly grain |
| **Data Modeling** | Star schema in Power Pivot with a Calendar dimension and one-to-many relationships to 5 fact tables |
| **DAX** | Measures for average spot rate, non-fuel cost, margin per mile (with blank-period handling) and margin % |
| **CUBEVALUE** | KPI cards that read measures straight from the data model and respond to slicers |
| **PivotTables & PivotCharts** | Yearly and monthly analysis by trailer type (Dry Van, Refrigerated, Flatbed) |
| **Dashboard Design** | Synced slicers, KPI cards, dynamic titles, conditional formatting, custom number formats |


## Data Sources

- [**U.S. Bureau of Transportation Statistics (BTS)** – Supply Chain and Freight Indicators.](https://catalog.data.gov/dataset/supply-chain-and-freight-indicators?from_hint=eyJxIjoic3VwcGx5IGNoYWluIn0%3D) <!-- link -->
- [**ATRI** – Operational Costs of Trucking, cost per mile by component, 2017–2025.](https://truckingresearch.org/2025/07/an-analysis-of-the-operational-costs-of-trucking-2025-update/) <!-- link -->

## Process

### 1. Data Cleaning (Power Query)
<!-- 2–3 lines + screenshot of the Applied Steps panel. -->
After defining the main indicators for the analysis, I:
1. **Split the source into one query per indicator**, filtering only the series used in the analysis: goods imports, new for-hire carriers, truck spot rates and diesel prices.
2. **Added operating cost data from a second source**, ATRI's cost per mile by component, loaded from a separate workbook.
3. **Standardized data types and structure across all tables**, so each one could feed PivotTables and connect to a shared Calendar table in the data model.

<img width="1918" height="848" alt="power_query" src="https://github.com/user-attachments/assets/79daf268-05e0-4812-871f-7b1d47447912" />


### 2. Data Model (Power Pivot)
<!-- Screenshot of the diagram view. Calendar should show the "1" side of every relationship. -->
I built a Data Model in Power Pivot in which the 'DATE' column of all my tables were connected using a Calendar Table. This allowed me to: 

- Easily analyze data from different tables in a single PivotTable.
- Combine weekly, monthly and annual sources in the same visuals.
- Use date slicers that work for all tables.
 
<img width="1004" height="590" alt="data_model" src="https://github.com/user-attachments/assets/343cf2ef-c681-474b-907a-ab3fb148b6d8" />


### 3. DAX Measures
I used DAX measures to:
- Calculate custom indicators (Margin per mile, Margin % and Average cost excluding fuel).
- Adapt values to changes in the slicers.
- Feed the dashboard's KPI cards. 

**Margin per mile measure**:
```dax
Margin per Mile :=
IF ( ISBLANK ( [Avg Spot Rate] ), BLANK (),
     [Avg Spot Rate] - [Non-Fuel Cost] )
```
This measure substracts the fuel cost from the spot rate ONLY when an average spot rate data exists. 

**Margin per mile %**:
```dax
DIVIDE( [Margin], [avg spot rate])
```
<!-- 1 line explaining why the IF matters (rates end May 2024, costs go to 2025). Show 1–2 measures only. -->

### 4. Dashboard
![Dashboard](images/dashboard.png)
<!-- Optional second GIF: changing trailer type and seeing the KPIs update. -->

## Limitations

- Spot rate data ends in **May 2024**, so 2024 covers January–May only.
- ATRI costs are annual; each month uses its year's cost.
- Margins compare linehaul spot rates against ATRI industry-average non-fuel costs.
- The analysis shows correlation, not causation.


## How to Use

Download `analysis.xlsx` and open it in Excel for Windows (Power Pivot required). Use the slicers on the **Dashboard** sheet.

## Author

**Mariano Morales** · [LinkedIn](URL) · [Email](mailto:...)
