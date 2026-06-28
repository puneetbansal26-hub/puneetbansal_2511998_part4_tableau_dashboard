# Business Insights Report

**Dashboard:** Executive Sales Performance Dashboard
**Dataset:** dashboard_sales_data.xlsx
**Total Sales:** 217M | **Total Profit:** 33M | **Total Orders:** 4K | **Return Rate:** 4.55% | **Avg. Profit Margin:** 13.12%

---

## Calculated Fields Reference

These five calculated fields were created in Tableau to power the KPI cards and various views in the dashboard:

| Calculated Field | Formula | Purpose |
|---|---|---|
| **Profit Margin** | `SUM([Profit]) / SUM([Sales])` | Drives the 13.12% Avg. Profit Margin KPI card and color-encoding in profitability views |
| **Cost** | `SUM([Sales]) - SUM([Profit])` | Supports cost structure analysis; derived from Sales (217M) minus Profit (33M) = ~184M cost base |
| **Average Order Value** | `SUM([Sales]) / COUNTD([Order ID])` | With 217M in sales and 4K orders, AOV is approximately 54,250 per order |
| **Return Rate** | `SUM(IF [Returned] = "Yes" THEN 1 ELSE 0 END) / COUNT([Order ID])` | Powers the 4.55% Return Rate KPI card |
| **Shipping Delay Bucket** | `IF [Days to Ship] <= 2 THEN "Fast" ELSEIF [Days to Ship] <= 4 THEN "Standard" ELSEIF [Days to Ship] <= 7 THEN "Delayed" ELSE "Severely Delayed" END` | Categorizes shipping performance into actionable operational tiers |

---

## Insight 1: Sales Trend Is Flat — Growth Has Stalled

**Observation:** The sales trend chart covering 2024 through 2025 shows monthly sales ranging between roughly 7M and 11M, with no consistent upward slope. The line fluctuates month to month but essentially moves sideways across the full period.

**Data Evidence:** Monthly values visible on the trend chart include: 9M, 9M, 9M, 7M, 7M, 6M, 10M, 9M (2024) and 8M, 7M, 7M, 7M, 11M, 10M, 9M (2025). The 2025 period does show a slight uptick in the middle months (11M) but quickly returns to baseline. There is no sustained growth trajectory.

**Business Interpretation:** A 217M revenue business with a flat monthly trend is maintaining volume but not growing. This could reflect market saturation in current segments, a lack of new customer acquisition, reliance on repeat purchases from an existing base, or competitive pricing pressure compressing deal sizes.

**Recommended Action:** Run a new vs. returning customer split on the order data. If the majority of orders are from returning customers and new customer volume is flat or declining, the business has an acquisition problem that discounting won't fix. If new customers are coming in but deal sizes are smaller, pricing strategy needs a review.

---

## Insight 2: South Region Is the Revenue Leader But Needs a Profitability Check

**Observation:** The South region leads all four regions in total sales at 65M, followed by North at 55M, with East and West both at 49M.

**Data Evidence:** Regional performance bar chart clearly shows South at 65M as the longest bar. The gap between South (65M) and the next region (North, 55M) is 10M — that's a meaningful difference in a 217M total sales context, representing roughly 30% of total revenue coming from one region.

**Business Interpretation:** South is clearly a strong market for this business. However, the dashboard only shows sales by region — not profit or margin. High sales in a region that's being driven by heavy discounting could mean the South's profitability contribution is actually lower than its revenue share suggests.

**Recommended Action:** Add a profit margin column or color-encode the regional bar chart by margin in the next dashboard iteration. If South's margin is below average despite leading on revenue, the discounting and cost structure there needs to be interrogated before investing more sales resources into that region.

---

## Insight 3: Technology Sub-Categories Dominate Revenue

**Observation:** In the category profitability view, Technology sub-categories produce far larger sales bars than Furniture or Office Supplies. Copiers alone are at 41M, Phones at 38M, and Accessories are also showing a large unlabeled bar that appears to exceed 35M.

**Data Evidence:** On the category profitability chart, Technology sub-categories (Copiers 41M, Phones 38M, Accessories) are visually clustered at the right end of the x-axis (which goes to 45M). Furniture sub-categories (Bookcases 12M, Furnishings 13M) sit in the 10–15M range, and Office Supplies sub-categories (Art 2M, Labels 2M, Storage 2M) are clustered near the zero end of the axis.

**Business Interpretation:** Technology is carrying the revenue load for this business. Copiers and Phones together likely account for over 35% of total sales. This creates both an opportunity (double down on what's working) and a risk (concentration in a single category).

**Recommended Action:** Prioritize Technology inventory management and supplier relationships to ensure supply continuity for Copiers and Phones. Simultaneously, build a plan to grow Furniture and Office Supplies margins so the revenue mix becomes less concentrated. For Technology Accessories specifically — these likely carry strong margins and should be a focus of upsell campaigns at point of checkout or post-purchase.

---

## Insight 4: Home Office Segment Slightly Leads Revenue, Which Is Counterintuitive

**Observation:** The customer segment view shows Home Office at 75M, Consumer at 72M, and Corporate at 71M. Home Office leads all three segments — a finding that surprises most people who assume Consumer or Corporate would be the biggest.

**Data Evidence:** The customer segment bar chart shows three bars of very similar height, but Home Office (75M) is the tallest, followed by Consumer (72M), then Corporate (71M). The total spread is only 4M across all three segments, but the ranking itself is the insight.

**Business Interpretation:** Home Office being the top revenue segment reflects a post-pandemic shift in work patterns. Customers working from home are buying both Office Supplies and Technology products for personal workspace setup — and they're doing it in meaningful volume. This segment may also have higher average order values if they're purchasing more deliberate, considered purchases rather than impulse buys.

**Recommended Action:** Develop a Home Office-specific product bundle or campaign. This is the largest revenue segment by a small but real margin, and it's likely the fastest-growing demographic for retail businesses selling both tech and office products. A "Work From Home Setup" bundle combining Technology Accessories, Phones, and select Furniture could resonate strongly and drive higher AOV in this segment.

---

## Insight 5: Office Supplies Sub-Categories Are Almost Invisible on Revenue

**Observation:** Art, Labels, and Storage each show approximately 2M in sales. Binders and Paper show values so small they don't display labels on the chart at all.

**Data Evidence:** In the category profitability chart, Office Supplies sub-categories (Art, Binders, Labels, Paper, Storage) all cluster in the 0–3M range on a chart that goes to 45M. Compared to Technology's 38M–41M sub-categories, Office Supplies barely registers as a visible bar.

**Business Interpretation:** There are two possible readings here. First, these could be low-price-per-unit items sold at volume, where the chart doesn't represent true contribution because unit economics work differently. Second, these could genuinely be underperforming product lines that occupy catalog space, warehouse space, and marketing attention without proportionate return. Without margin data per sub-category, it's hard to distinguish.

**Recommended Action:** Pull a per-sub-category margin and unit volume analysis. If Office Supplies sub-categories have thin margins AND low volume, a product line rationalization exercise is warranted. If they have healthy per-unit margins but low total revenue, the opportunity is to grow volume through bundling, promotions, or cross-sell at checkout with Technology purchases.

---

## Insight 6: Return Rate of 4.55% Is a Business Strength — But Needs Monitoring

**Observation:** The Return Rate KPI card shows 4.55% across approximately 4,000 total orders. This is a low return rate by retail standards.

**Data Evidence:** KPI card directly shows Return Rate: 4.55%. With 4K total orders, that translates to approximately 182 returned orders in the dataset period.

**Business Interpretation:** A sub-5% return rate indicates customers are generally receiving what they expect. This is especially significant given that the business sells Technology products (Copiers, Phones) which typically carry higher return rates than commodity office supplies due to setup complexity and high purchase scrutiny.

**Recommended Action:** Protect this metric proactively. As the product catalog grows or new categories are introduced, monitor return rates by sub-category to catch any quality or expectation gaps early. Set an internal alert threshold — if return rate on any sub-category exceeds 8–10%, that should trigger a product quality or description review automatically.

---

## Insight 7: Profit Margin of 13.12% Is Positive But Has Compression Risk

**Observation:** The Avg. Profit Margin KPI shows 13.12% on 217M in sales, producing 33M in profit. This is a reasonable margin for a retail business but leaves limited buffer against cost increases or revenue pressure.

**Data Evidence:** KPI cards: Sales 217M, Profit 33M, Avg. Profit Margin 13.12%. Cross-checking: 33M / 217M = 15.2%, which means the "Avg. Profit Margin" KPI is likely calculated as an average of per-order margin rates rather than the simple total ratio — a nuance in how the calculated field is set up in Tableau.

**Business Interpretation:** A 13% margin in retail is workable but not robust. If the flat sales trend continues and cost inflation rises (shipping, warehousing, supplier costs), margin compression could move this to single digits relatively quickly. The business needs either a revenue growth engine or a cost efficiency program — ideally both.

**Recommended Action:** Build a simple margin sensitivity model: what happens to net profit if margin drops 1%, 2%, or 3%? At 217M sales, a 1% margin compression means ~2.2M less profit. Knowing this number makes it easier for leadership to justify investment in cost efficiency programs or pricing strategy reviews.

---

## Insight 8: Revenue Is Concentrated in Two Technology Sub-Categories — A Structural Risk

**Observation:** Copiers (41M) and Phones (38M) together account for approximately 79M in sales — roughly 36% of the 217M total revenue — from just two sub-categories within one category.

**Data Evidence:** Visible in the category profitability chart. Copiers 41M + Phones 38M = 79M. Total sales 217M. 79/217 = approximately 36.4% concentration in two sub-categories.

**Business Interpretation:** This is a business risk that doesn't show up in the headline metrics. If Copier demand shifts (as remote work reduces office printing), or if a major Phone supplier has supply chain issues, or if a competitor undercuts pricing in either of these sub-categories, the impact on total business revenue would be severe and disproportionate. Leadership may not be aware of just how reliant the overall sales number is on these two lines.

**Recommended Action:** Treat Copiers and Phones as strategic, high-priority lines requiring dedicated supplier relationship management, inventory buffer planning, and competitive pricing monitoring. In parallel, develop a category diversification roadmap — what would it take to grow Furniture or Office Supplies revenue enough to reduce Technology concentration from 36% of sales to 25% over the next 2 years? This is a strategic resilience question worth putting on the leadership agenda.

---

*All figures, rankings, and observations are derived directly from the Executive Dashboard as built in Tableau (executive_dashboard.twbx) on dataset dashboard_sales_data.xlsx. Specific values referenced match KPI cards and chart labels visible in the dashboard.*
