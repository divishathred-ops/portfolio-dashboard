# Tableau Dashboard — 8 KPIs and 8 Graphs Build Guide

**File:** `Gundavarapu_Divishath_tableau_dashboard.twb`  
**Data source:** `Fact_Projects_Merged-3.xlsx`  
**Built in:** Tableau Desktop 2025.2

---

## Global Setup (do this before building anything)

1. Connect to `Fact_Projects_Merged-3.xlsx` as the data source.
2. Go to `Format → Workbook → Lines` and set Grid Lines, Zero Lines, Axis Rulers, Axis Ticks, Drop Lines all to **None**.
3. Go to `Format → Workbook → Shading` and set Worksheet, Pane, Header all to **None**.

---

## Part 1 — The 8 KPI Cards

---

### KPI 1 — Total Actual Cost
1. New worksheet → drag `Actual_Cost` to the **Text** square on the Marks card.
2. Right-click the pill → Format → Numbers → Currency (Custom) → `$`, 0 decimal places.
3. Click Text square → set font size **36pt bold**.
4. Name sheet: `KPI 1 - Actual Cost`

---

### KPI 2 — Overall Cost Variance
1. New worksheet → drag `Cost_Variance` to **Text**.
2. Right-click Data pane → **Create Calculated Field** → name it `Global Variance Color`:
```
IF SUM([Cost_Variance]) > 0 THEN "Red" ELSE "Green" END
```
3. Drag `Global Variance Color` to **Color**.
4. Set font size 36pt bold.
5. Name sheet: `KPI 2 - Cost Variance`

---

### KPI 3 — Global Duration Delay
1. New worksheet → drag `Duration_Variance` to **Text**.
2. Set font size 36pt bold.
3. Name sheet: `KPI 3 - Duration Delay`

---

### KPI 4 — Deliverables Success Rate
1. New worksheet → right-click Data pane → **Create Calculated Field** → name it `Success Rate`:
```
SUM([Actual_Deliverables]) / SUM([Planned_Delivrable])
```
2. Drag `Success Rate` to **Text**.
3. Right-click pill → Format → Numbers → Percentage, 0 decimals.
4. Set font size 36pt bold.
5. Name sheet: `KPI 4 - Success Rate`

---

### KPI 5 — Most Critical Country
1. New worksheet → drag `Country` to **Text**, drag `Cost_Variance` to **Detail**.
2. Sort descending by Cost_Variance so the worst country is first.
3. Right-click Data pane → **Create Calculated Field** → name it `Top 1 Formula`:
```
INDEX() = 1
```
4. Drag `Top 1 Formula` to **Filters** → check True → OK.
5. Set font size 36pt bold.
6. Name sheet: `KPI 5 - Critical Country`

---

### KPI 6 — Total Tracked Phases
1. New worksheet → right-click drag `Phase` to **Text** → select **CNTD(Phase)**.
2. Set font size 36pt bold.
3. Name sheet: `KPI 6 - Tracked Phases`

---

### KPI 7 — Total Active Projects
1. New worksheet → right-click drag `Project_ID` to **Text** → select **CNTD(Project_ID)**.
2. Set font size 36pt bold.
3. Name sheet: `KPI 7 - Total Projects`

---

### KPI 8 — Portfolio Health Score (At Risk + Over Budget count)
1. New worksheet → right-click drag `Project_ID` to **Text** → select **CNTD(Project_ID)**.
2. Drag `Health_Status` to **Filters** → check only `At Risk` and `Over Budget` → OK.
3. Set font size 36pt bold.
4. Name sheet: `KPI 8 - Projects at Risk`

---

## Part 2 — The 8 Graphs

---

### Graph 1 — Deliverables Planned vs Actual (Side-by-Side Bar)
1. Drag `Phase` to **Columns**.
2. Drag `Measure Values` to **Rows**.
3. Drag `Measure Names` to **Filters** → keep only `Planned_Delivrable` and `Actual_Deliverables`.
4. Drag `Measure Names` from the Data pane to **Columns** (right of Phase) to unstack bars.
5. Drag `Measure Names` to **Color**.
6. Name sheet: `Graph 1 - Deliverables`

---

### Graph 2 — Geographical Variance Mapping (Top/Bottom 10 Horizontal Bar)
1. Drag `Country` to **Rows**, drag `Cost_Variance` to **Columns**.
2. Create Calculated Field → name it `Top and Bottom 10`:
```
RANK(SUM([Cost_Variance]), 'desc') <= 10
OR RANK(SUM([Cost_Variance]), 'asc') <= 10
```
3. Drag `Top and Bottom 10` to **Filters** → check True.
4. Drag `Cost_Variance` to **Color** → Edit Colors → Custom Diverging → center at 0.
5. Name sheet: `Graph 2 - Geo Variance`

---

### Graph 3 — The Efficiency Matrix (Scatter Plot)
1. Drag `Actual_Duration` to **Columns**, drag `Cost_Variance_Pct` to **Rows**.
2. Drag `Project_ID` to the **Detail** square to explode to individual points.
3. Change Marks dropdown to **Circle**.
4. Drag `Health_Status` to **Color**.
5. Click Color → set Opacity to 70%.
6. Name sheet: `Graph 3 - Efficiency Scatter`

---

### Graph 4 — Duration Creep by Phase (Side-by-Side Averages)
1. Drag `Phase` to **Columns**, drag `Measure Values` to **Rows**.
2. Drag `Measure Names` to **Filters** → keep only `Planned_Duration` and `Actual_Duration`.
3. Right-click both pills on the Measure Values card → Measure → **Average**.
4. Drag `Measure Names` to **Columns** (next to Phase) to unstack.
5. Drag `Measure Names` to **Color**.
6. Name sheet: `Graph 4 - Duration Creep`

---

### Graph 5 — Execution vs Promise (Dumbbell Chart)
1. Drag `Project_Type` to **Columns**, drag `Measure Values` to **Rows**.
2. Drag `Measure Names` to **Filters** → keep only `Planned_Delivrable` and `Actual_Deliverables`.
3. Change Marks card to **Circle**. Drag `Measure Names` to **Color**.
4. Hold Ctrl and drag the `Measure Values` pill on Rows to duplicate it next to itself.
5. On the second Measure Values Marks card → change type to **Line** → drag `Measure Names` to **Path**.
6. Right-click the second `Measure Values` pill → **Dual Axis** → right-click right axis → **Synchronize Axis**.
7. Name sheet: `Graph 5 - Dumbbell`

---

### Graph 6 — Cost Variance Trend (Timeline Area Chart)
1. Drag `Date` to **Columns** → right-click pill → select **Month (Continuous)**.
2. Drag `Cost_Variance` to **Rows**.
3. Change Marks card to **Area**.
4. Drag `Cost_Variance` to **Color** → Edit Colors → Custom Diverging → center at 0.
5. Set Opacity to 50%.
6. Name sheet: `Graph 6 - Variance Trend`

---

### Graph 7 — Phase Health Distribution (100% Stacked Bar)
1. Drag `Phase` to **Rows**, drag `Project_ID` to **Columns** → change to **Count Distinct**.
2. Drag `Health_Status` to **Color**.
3. Right-click `CNTD(Project_ID)` pill → **Quick Table Calculation** → **Percent of Total**.
4. Right-click again → **Compute Using** → **Table (Across)**.
5. Name sheet: `Graph 7 - Health Distribution`

---

### Graph 8 — Custom Threshold Outliers (Red/Green Bar)
1. Drag `Project_Type` to **Columns**, drag `Cost_Variance` to **Rows**.
2. Create Calculated Field → name it `Variance Color Flag`:
```
IF SUM([Cost_Variance]) > 65000 THEN "Out of Bounds"
ELSEIF SUM([Cost_Variance]) < -35000 THEN "At Risk"
ELSE "On Track"
END
```
3. Drag `Variance Color Flag` to **Color**.
4. Name sheet: `Graph 8 - Threshold Outliers`

---

## Part 3 — Dashboard Assembly

1. Open a new **Dashboard** tab.
2. Set Dashboard Size to **Fixed 1920 × 1080**.
3. Drag a **Vertical Container** onto the canvas.
4. Drag the 8 KPI sheets to the top row.
5. Drag the 8 Graph sheets below in a 2-row layout.
6. Right-click each sheet background → Format → Shading → Worksheet: **None**.
7. Enable **Filter Actions** between all sheets: Dashboard → Actions → Add Action → Filter.
