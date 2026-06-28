# Executive Sales Performance Dashboard — Part 4

**Dataset:** `data/dashboard_sales_data.xlsx`
**Tableau Workbook:** `tableau/executive_dashboard.twbx`

---

## Business Problem Summary

The retail leadership team needed a single, interactive dashboard to monitor business performance without jumping between spreadsheets or waiting for monthly static reports. The core ask: show sales performance, profitability, customer segment behavior, category contribution, regional spread, and operational metrics like return rate and shipping — all in one executive-facing view with filters that let leadership slice the data themselves.

The harder part wasn't building the charts — it was making them tell a coherent story. The dashboard was designed to lead with headline KPIs, then allow drill-down into regional, category, and segment-level detail, so leadership can start broad and go narrow based on what catches their attention.

---

## Dataset Description

**File:** `data/dashboard_sales_data.xlsx`
**Source:** Course-provided dataset from Part 4 shared Drive folder

The dataset is a multi-year retail transactional dataset. Key fields:

**Date Fields:**
- `Order Date` — when the order was placed
- `Ship Date` — when the order was dispatched
- `Days to Ship` — difference between order date and ship date (used for Shipping Delay Bucket)

**Geographic Fields:**
- `Region` — four regions: East, North, South, West
- `State`, `City`, `Postal Code`

**Categorical Fields:**
- `Category` — Furniture, Office Supplies, Technology
- `Sub-Category` — 17 sub-categories including Bookcases, Chairs, Furnishings, Tables (Furniture); Art, Binders, Labels, Paper, Storage (Office Supplies); Accessories, Copiers, Machines, Phones (Technology)
- `Customer Segment` — Consumer, Corporate, Home Office
- `Ship Mode` — Standard Class, Second Class, First Class, Same Day

**Numerical Measures:**
- `Sales`, `Profit`, `Discount`, `Quantity`

**Binary/Flag Fields:**
- `Returned` — Yes/No flag for whether the order was returned

**Assumptions Made:**
- `Days to Ship` was calculated in Tableau as `DATEDIFF('day', [Order Date], [Ship Date])` where not pre-existing in the dataset.
- Null values in the `Returned` field were treated as "No" (not returned) for return rate calculations.
- Sales and Profit figures are denominated in the same currency throughout (no multi-currency adjustment needed).
- Discount values appear as decimals (0.2 = 20%) — this was accounted for in all calculated field logic.
- Orders with negative profit were retained as valid data representing discounted or loss-making transactions, not data entry errors.

---

## Tableau Workbook Description

**File:** `tableau/executive_dashboard.twbx`

The packaged workbook embeds the dataset, all calculated fields, all supporting worksheet views, and the final executive dashboard. It opens correctly in Tableau Desktop or Tableau Public without requiring a separate data source reconnection.

**Workbook Contents:**
- 5 individual worksheet views (Sales Trend, Regional Performance, Category Profitability, Customer Segment, and KPI Card)
- 1 executive dashboard combining all views
- 7 calculated fields
- Interactive filters responding across all dashboard sheets
- KPI cards for 5 headline metrics

---

## Calculated Fields Created

All calculated fields were created inside Tableau Desktop via Analysis → Create Calculated Field.

| Field Name | Formula | Notes |
|---|---|---|
| **Profit Margin** | `SUM([Profit]) / SUM([Sales])` | Formatted as %. Dashboard shows 13.12% overall. ZN() wrapper avoids divide-by-zero. |
| **Cost** | `SUM([Sales]) - SUM([Profit])` | With 217M sales and 33M profit, cost base is approximately 184M. |
| **Average Order Value** | `SUM([Sales]) / COUNTD([Order ID])` | With 217M sales and 4K orders, AOV is approximately 54,250. COUNTD prevents double-counting multi-row orders. |
| **Return Rate** | `SUM(IF [Returned] = "Yes" THEN 1 ELSE 0 END) / COUNT([Order ID])` | Dashboard shows 4.55%. Null Returned values treated as 0. |
| **Shipping Delay Bucket** | `IF [Days to Ship] <= 2 THEN "Fast" ELSEIF [Days to Ship] <= 4 THEN "Standard" ELSEIF [Days to Ship] <= 7 THEN "Delayed" ELSE "Severely Delayed" END` | Thresholds based on retail shipping norms. Powers shipping performance analysis. |
| **Discount Tier** | `IF [Discount] = 0 THEN "No Discount" ELSEIF [Discount] <= 0.10 THEN "1-10%" ELSEIF [Discount] <= 0.20 THEN "11-20%" ELSE "20%+" END` | Buckets continuous discount values for categorical analysis. |
| **Year of Order** | `YEAR([Order Date])` | Extracted date part used for year-level filters and year-over-year comparison in trend view. |

---

## Dashboard Components

### KPI Cards (Top-Left)
Five headline metrics displayed as a summary text table:
- **Sales: 217M**
- **Profit: 33M**
- **Total Orders: 4K**
- **Return Rate: 4.55%**
- **Avg. Profit Margin: 13.12%**

These KPI cards respond to all dashboard-level filters so the numbers always reflect the current filter selection.

### Sales Trend View (Top-Center)
- **Chart type:** Line chart
- **Dimensions:** Order Date by Month (X-axis), SUM([Sales]) in millions (Y-axis)
- **Period shown:** 2024 and 2025, with year divider
- **Monthly values:** Range from approximately 6M to 11M
- **Story:** Flat trend — no sustained upward slope across the period

### Regional Performance View (Top-Right)
- **Chart type:** Horizontal bar chart
- **Dimension:** Region (East, North, South, West)
- **Measure:** SUM([Sales])
- **Values:** East 49M, North 55M, South 65M, West 49M
- **Story:** South leads at 65M; East and West are tied at 49M

### Category Profitability View (Bottom-Left)
- **Chart type:** Horizontal bar chart with nested row hierarchy
- **Dimensions:** Category → Sub-Category (rows), SUM([Sales]) (columns)
- **Notable values:** Copiers 41M, Phones 38M, Furnishings 13M, Bookcases 12M, Art/Labels/Storage ~2M each
- **Story:** Technology sub-categories dominate; Office Supplies barely register

### Customer Segment View (Bottom-Right)
- **Chart type:** Vertical bar chart
- **Dimension:** Customer Segment (Consumer, Corporate, Home Office)
- **Measure:** SUM([Sales])
- **Values:** Consumer 72M, Corporate 71M, Home Office 75M
- **Story:** Segments are nearly equal; Home Office leads by a small margin

---

## Filters and Interactions Used

**Dashboard-level filters applied across all sheets:**
- Region (multi-select dropdown)
- Category (multi-select dropdown)
- Customer Segment (multi-select)
- Order Date range (date filter)
- Ship Mode (multi-select)

**Filter Actions (Dashboard → Actions → Filter):**
- Clicking a region bar in the Regional Performance view filters the Category and Segment views to show only that region's data
- Clicking a category bar in the Category Profitability view filters the Trend and Segment views to that category
- Selecting a customer segment bar filters all other charts to that segment's data

"Clear the selection will" option set to **Show all values** across all actions — this ensures that clicking away from a selection resets the full dashboard rather than leaving it in a partially filtered state.

---

## Key Business Insights

Eight insights documented in full in `outputs/business_insights.md`. Summary:

1. **Sales trend is flat (2024–2025)** — Monthly range of 7M–11M with no upward slope; growth has stalled
2. **South leads revenue at 65M** — But no margin data available yet to confirm profitability quality
3. **Technology dominates category revenue** — Copiers (41M) and Phones (38M) together are ~36% of total sales
4. **Home Office is the top segment at 75M** — Slightly ahead of Consumer (72M) and Corporate (71M); a post-pandemic trend
5. **Office Supplies sub-categories are near-invisible** — Art, Labels, Storage at ~2M each; possible rationalization candidates
6. **Return rate of 4.55% is a business strength** — Below typical retail benchmarks; needs active protection
7. **Profit margin of 13.12% has compression risk** — Flat revenue + rising costs = margin erosion over time
8. **Revenue concentration in 2 sub-categories is a structural risk** — Copiers + Phones = ~36% of 217M total sales

---

## Dashboard Story Summary

Full narrative in `outputs/dashboard_story.md`. One-paragraph version:

The business is generating 217M in sales with a 13.12% average profit margin and a healthy 4.55% return rate across 4K orders. However, the sales trend is flat across 2024–2025, which is the dashboard's most important warning signal. South is the strongest region by revenue but needs a profit margin check. Technology — specifically Copiers and Phones — is carrying a disproportionate share of total revenue, creating concentration risk. Home Office is the slightly leading customer segment at 75M. Office Supplies sub-categories are extremely low revenue and should be evaluated for rationalization or growth investment. The immediate priorities are: diagnosing the flat trend, protecting and growing Technology Accessories margins, building a Home Office segment strategy, and adding profitability data to the regional view.

---

## Assumptions and Limitations

**Assumptions:**
- Null values in `Returned` field treated as "not returned"
- Shipping delay bucket thresholds (2/4/7 days) based on general retail norms, not company-specific SLAs
- All financial figures assumed to be in a single consistent currency (USD)
- `Days to Ship` derived from Order Date and Ship Date where not pre-calculated

**Limitations:**
- Regional view shows sales only — no profit margin by region visible in current dashboard
- Sales trend view does not have category or segment overlay — can't see what's driving monthly movements
- Sub-category labels for smaller bars (Chairs, Tables, Binders, Paper, Accessories) are not fully labeled on the chart
- Customer segment view shows sales only — no AOV, margin, or return rate by segment
- No discount analysis view in current dashboard iteration
- Return rate shown as aggregate KPI only — no breakdown by category, segment, or region visible

---

## Screenshots Included

| File | What It Shows |
|---|---|
| `screenshots/full_dashboard.png` | Complete executive dashboard with KPI card, trend view, regional view, category view, and segment view |
| `screenshots/sales_trend_view.png` | Sales trend line chart showing 2024–2025 monthly performance (7M–11M range) |
| `screenshots/regional_performance_view.png` | Regional bar chart: South 65M, North 55M, East 49M, West 49M |
| `screenshots/category_profitability_view.png` | Sub-category bar chart: Copiers 41M, Phones 38M, Furnishings 13M, Bookcases 12M, Office Supplies ~2M each |
| `screenshots/filter_interaction_view.png` | Dashboard with a filter applied (e.g., South region selected), showing all views updating in response |

Screenshots captured from Tableau Desktop. All charts and labels are clearly readable.

---

## Repository Structure

```
part4_tableau_dashboard/
├── data/
│   └── dashboard_sales_data.xlsx
├── tableau/
│   └── executive_dashboard.twbx
├── outputs/
│   ├── dashboard_story.md
│   ├── business_insights.md
│   └── chart_selection_justification.md
├── screenshots/
│   ├── full_dashboard.png
│   ├── sales_trend_view.png
│   ├── regional_performance_view.png
│   ├── category_profitability_view.png
│   └── filter_interaction_view.png
└── README.md
```

---

*Part 4 submission — Executive Dashboard & Data Storytelling. All documentation files generated based on the executive_dashboard.twbx Tableau workbook built on dashboard_sales_data.xlsx.*
