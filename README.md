# NHS Dashboard — Data Analysis Project

**Repository:** `DashBoard_NHS`
**Author:** Perumal V  — Data Analyst
---

## Project overview

You are building a multi-page Power BI dashboard (3–4 pages) for the UK National Health Service (NHS). The dashboard uses the repository tables as a *star schema* where **`Visits`** is the central fact table and other tables act as dimensions. The dashboard includes KPI cards, time-series trends, demographic breakdowns, provider performance, cost analytics, and geographic insights.

---

## Quick file / folder suggestions

Place your exported dashboard images and any supporting assets here:

```
/images/
  └─ dashboard_overview.png   # (add screenshot(s) of your Power BI pages here)
/docs/
  └─ data-dictionary.md       # optional: detail each table & column
README.md                     # this file
```

## Dashboard Previews

<p align="center">
  <img src="https://github.com/Perumal2003/DashBoard_NHS/blob/main/View/NHS%201.png" alt="Dashboard 1" width="300"/>
  <img src="https://github.com/Perumal2003/DashBoard_NHS/blob/main/View/NHS%202.png" alt="Dashboard 2" width="300"/>
  <img src="https://github.com/Perumal2003/DashBoard_NHS/blob/main/View/NHS%203.png" alt="Dashboard 3" width="300"/>
</p>

<p align="center">
  <img src="https://github.com/Perumal2003/DashBoard_NHS/blob/main/View/NHS%204.png" alt="Dashboard 4" width="300"/>
  <img src="https://github.com/Perumal2003/DashBoard_NHS/blob/main/View/NHS%205.png" alt="Dashboard 5" width="300"/>
</p>

---

## Data model & relationships (Power BI)

Treat **`Visits`** as the fact table. The relationships you should define are **One-to-Many** where the *dimension* table is on the **one** side and **Visits** is on the **many** side. Example relationships:

* `Patients[PatientID] (1)` -> `Visits[PatientID] (*)`
* `Providers[ProviderID] (1)` -> `Visits[ProviderID] (*)`
* `Departments[DepartmentID] (1)` -> `Visits[DepartmentID] (*)`
* `Rooms[RoomID] (1)` -> `Visits[RoomID] (*)`
* `Medications[MedicationID] (1)` -> `Visits[MedicationID] (*)` *(if visits store medication rows; if a visit can have many medications, use a bridge table `VisitMedications`)*
* `Procedures[ProcedureID] (1)` -> `VisitProcedures[ProcedureID] (*)` -> `Visits[VisitID] (*)` *(use a bridge if many-to-many)*

> **Note:** If Visits contains repeating lists (e.g., multiple procedures or meds in a single row), normalize into bridge tables so relationships remain clear and Power BI filters propagate correctly.

---

## Key Performance Indicators (KPIs)

Display KPI cards at the top of the first page. Recommended KPIs and example DAX measures are below.

**Recommended KPI cards**

* Total Visits
* Total Revenue from Visits
* Total Treatment Cost
* Total Medication Cost
* Total Room Charges
* Average Patient Satisfaction Score

---

## DAX measures (examples)

Use these DAX formulas directly in Power BI (adjust column/table names to match your model):

```dax
-- Total number of visits
Total Visits = COUNTROWS(Visits)

-- Total revenue from visits (assuming Visits[VisitRevenue])
Total Revenue = SUM(Visits[VisitRevenue])

-- Total treatment cost
Total Treatment Cost = SUM(Visits[TreatmentCost])

-- Total medication cost
Total Medication Cost = SUM(Visits[MedicationCost])

-- Total room charges
Total Room Charges = SUM(Visits[RoomCharge])

-- Average patient satisfaction (assuming Visits[SatisfactionScore])
Avg Satisfaction = AVERAGE(Visits[SatisfactionScore])

-- Total cost (treatment + medication + room)
Total Cost = [Total Treatment Cost] + [Total Medication Cost] + [Total Room Charges]

-- Total cost by department
Total Cost by Dept = CALCULATE([Total Cost], ALLEXCEPT(Departments, Departments[DepartmentID]))

-- Visits over time (monthly)
Visits (Month) =
CALCULATE([Total Visits],
    DATESINPERIOD('Date'[Date], LASTDATE('Date'[Date]), -12, MONTH)
)
```

> Tip: create a dedicated `Date` table, mark it as the Date table in Power BI and use it for all time intelligence calculations (month, quarter, year).

---

## Dashboard pages & recommended visuals

Below is a suggested 4‑page layout and the visuals to include on each page.

### Page 1 — Executive / Overview

* KPI cards (Total Visits, Total Revenue, Total Costs, Avg Satisfaction)
* Time-series: Visits by Month (line chart)
* Revenue & Cost comparison (clustered column or combo chart)
* Map: Visits by City (choropleth or bubble map)

### Page 2 — Patient Demographics & Distribution

* Donut / stacked bar: Gender distribution
* Bar or treemap: Patients by City and by State
* Age distribution: Histogram or box plot
* Bar chart: Patients by Race
* Slicers: Age group, Gender, City, State

### Page 3 — Provider Performance

* Bar chart: Visits per Provider (top N with sorting)
* Card / KPI: Avg Satisfaction by Provider (sortable table)
* Scatter: Providers (Visits vs Avg Satisfaction) — add size by Revenue
* Table: Provider summary (Visits, Avg Satisfaction, Avg Cost)

### Page 4 — Cost & Time Trends

* Stacked column: Total cost by Department (treatment/medication/room stacked)
* Column: Room charges by Room Type
* Line chart: Monthly trend of Treatment Cost
* Line/area: Emergency vs Regular visits over time
* Table or map: Cities with highest treatment costs & procedure counts

---

## Geographical Insights

* Use a map visual (ArcGIS or built-in map) and a table for numeric precision.
* Create measures like `TotalTreatmentCostByCity = CALCULATE([Total Treatment Cost], ALLEXCEPT(Cities, Cities[CityName]))` and show both map bubbles and a top‑10 table.

---

## Interactivity & UX suggestions

* Add slicers for Date (range), City, Department, Provider, Visit Type (Emergency vs Regular), and Gender.
* Use bookmarks to provide quick filters/views (e.g., "Top Providers", "Cost Overview").
* Tooltips: include mini KPI measures in tooltips for richer hover info.
* Drillthrough: enable drillthrough from Department to Provider to individual Visit details.

---

## How to add your dashboard image to this README

1. Export a screenshot of your Power BI Page (PNG).
2. Put the file in `/images` inside the repository (name it `dashboard_overview.png` or similar).
3. Commit & push the file to GitHub.
4. When the file is in the repository the markdown below (already included earlier) will render the image in README:

```md
![Dashboard overview](images/dashboard_overview.png)
```

---

## Data quality & modeling notes

* Ensure numerical fields (costs, revenue) are numeric and cleaned for nulls/outliers.
* If a visit contains multiple medications or procedures, normalize to bridge tables: `VisitMedications`, `VisitProcedures`.
* Handle missing city/state by creating an `Unknown` category or excluding based on the analysis need.

---

## Extra: Example Analysis questions (to answer in the dashboard)

* Which providers have the highest number of visits and highest average satisfaction?
* Which cities/states have unexpectedly high treatment cost per visit?
* How do emergency visits trend vs regular visits across months and departments?
* What departments produce the largest share of total costs?

---


*End of README*
