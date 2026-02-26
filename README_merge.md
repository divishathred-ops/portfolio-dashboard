# Data Merging — How the Fact Table Was Built

**Notebook:** `Merging_the_columns.ipynb`  
**Platform:** Google Colab  
**Input:** `Tableau_project_1.xlsx` (cleaned workbook with 7 separate sheets)  
**Output:** `Fact_Projects_Merged-3.xlsx` (single merged fact table with calculated columns)

---

## Input Sheets and Their Contents

The cleaned workbook had 7 sheets, each holding a separate piece of the data:

| Sheet | Columns | Purpose |
|---|---|---|
| Sheet1 | Project_ID, Phase, Date, Planned_Duration, Planned_Cost, Planned_Delivrable | Base planned data (starting point) |
| Sheet2 | Project_ID, Project_Type | Project category lookup |
| Sheet3 | Project_ID, Phase, Actual_Cost | Actual cost per phase |
| Sheet4 | Project_ID, Phase, Actual_Duration | Actual duration per phase |
| Sheet5 | Project_ID, Phase, Actual_Deliverables | Actual deliverables per phase |
| Sheet6 | Project_ID, Country | Country per project |
| Sheet7 | Country, Region, Type | Region and type lookup by country |

---

## Step-by-Step Merge Logic

**Step 1 — Load all sheets**  
All 7 sheets were loaded into separate DataFrames using `pd.read_excel()`.

**Step 2 — Strip whitespace from column names**  
All column headers had `.str.strip()` applied to remove hidden spaces that break merges.

**Step 3 — Merge Sheet2 → base (Sheet1)**  
Join key: `Project_ID` (left join)  
Adds: `Project_Type`

**Step 4 — Merge Sheet3 → fact**  
Join keys: `Project_ID` + `Phase` (left join)  
Adds: `Actual_Cost`  
Two keys used because cost varies per project AND per phase.

**Step 5 — Merge Sheet4 → fact**  
Join keys: `Project_ID` + `Phase` (left join)  
Adds: `Actual_Duration`

**Step 6 — Merge Sheet5 → fact**  
Join keys: `Project_ID` + `Phase` (left join)  
Adds: `Actual_Deliverables`

**Step 7 — Merge Sheet6 → fact**  
Join key: `Project_ID` (left join)  
Adds: `Country`

**Step 8 — Merge Sheet7 → fact**  
Join key: `Country` (left join)  
Adds: `Region`, `Type`

---

## Column Reordering

After all merges, columns were reordered into this final structure:

```
Project_ID, Project_Type, Phase, Date, Country, Region, Type,
Planned_Duration, Planned_Cost, Planned_Delivrable,
Actual_Cost, Actual_Duration, Actual_Deliverables
```

---

## Calculated Columns Added

After merging, 5 derived columns were calculated:

| Column | Formula |
|---|---|
| Cost_Variance | Actual_Cost − Planned_Cost |
| Duration_Variance | Actual_Duration − Planned_Duration |
| Deliverables_Variance | Actual_Deliverables − Planned_Delivrable |
| Cost_Variance_Pct | (Cost_Variance / Planned_Cost) × 100, rounded to 2 decimals |
| Health_Status | On Track / At Risk / Over Budget (see logic below) |

**Health_Status logic:**
- `On Track` — Actual_Cost ≤ Planned_Cost AND Actual_Duration ≤ Planned_Duration
- `Over Budget` — Actual_Cost > Planned_Cost × 1.2
- `At Risk` — everything else

---

## Output

The final merged table was exported in two formats:
- `Fact_Projects_Merged.xlsx` — Excel file with a single sheet named `Fact_Projects`
- `Fact_Projects_Merged.csv` — CSV for direct use in the web dashboard

The file used in this project is `Fact_Projects_Merged-3.xlsx`.
