# Chart Selection Justification

**Dashboard:** Executive Sales Performance Dashboard
**Workbook:** executive_dashboard.twbx
**Views in Dashboard:** KPI Card, Sales Trend View, Regional Performance View, Category Profitability View, Customer Segment View

---

## Overview

This document justifies every chart type used in the Executive Dashboard. The decisions weren't arbitrary — each chart was chosen to answer a specific business question as clearly and quickly as possible for a leadership audience. The guiding test applied to each chart: *can a busy executive understand what this chart is saying in under 5 seconds without reading a lengthy caption?*

Where a chart choice might seem simple or obvious, there was usually an alternative that was considered and rejected. Those alternatives are documented in the "Mistake Avoided" section for each chart.

---

## Chart 1: KPI Card — Text Summary Table

**Business Question Answered:** What are the headline numbers for this business right now? At a glance, is the business healthy?

**Why This Chart Type:**
KPI cards (text-based summary metrics displayed prominently) are not really "charts" in the traditional sense — they're dashboard anchors. The five metrics shown are: Sales (217M), Profit (33M), Total Orders (4K), Return Rate (4.55%), and Avg. Profit Margin (13.12%). These are the five questions every executive asks first before looking at anything else. Displaying them as large, clearly labeled numbers in a dedicated card at the top-left gives leadership the "state of the union" in under 3 seconds.

**Field Mapping:**
- Sales: SUM([Sales]) → formatted as M (millions)
- Profit: SUM([Profit]) → formatted as M
- Total Orders: COUNTD([Order ID]) → formatted as K
- Return Rate: Return Rate calculated field → formatted as percentage
- Avg. Profit Margin: Profit Margin calculated field → formatted as percentage

**Design Principle Applied:**
Kept the KPI card format clean and tabular — label on the left, value right-aligned. No icons, no sparklines, no color coding on the KPI card itself. The simplicity means leadership can scan it in one pass. Adding too much decoration to a KPI card creates noise around what should be signal.

**Mistake Avoided:**
Did not use gauge charts or speedometer-style visuals for KPIs. These are visually engaging but extremely poor at conveying precise values — the human eye is bad at reading angles. A number that says "217M" is instantly understood; a gauge needle pointing somewhere in the green-yellow zone is not.

---

## Chart 2: Sales Trend View — Line Chart

**Business Question Answered:** How are total sales moving month by month across 2024 and 2025? Is there a growth trend, a seasonal pattern, or stagnation?

**Why This Chart Type:**
Line charts are the standard tool for continuous time-series data, and the reason is perceptual: the human eye naturally follows a line and reads its slope as direction and momentum. Monthly sales data is continuous — each month connects to the next — so a line chart correctly represents that continuity. The chart spans 2024 and 2025 with a vertical divider between years, making year-over-year comparison straightforward.

**Field Mapping:**
- X-axis: Order Date (by Month — abbreviated to F, A, J, A, O for Feb, Apr, Jun, Aug, Oct)
- Y-axis: SUM([Sales]) formatted in M (millions), ranging from 0M to 40M
- Year markers: 2024 and 2025 labeled above the trend line with a dividing reference line

**Design Principle Applied:**
Monthly value labels (9M, 8M, 10M etc.) were added directly on the line points rather than relying on axis gridlines alone. This is especially useful in executive dashboards where the audience may not pause to estimate values from the axis — the number is right there on the data point. The Y-axis goes to 40M even though values peak at ~11M, which gives some headroom and avoids the visual distortion of a tight axis.

**Mistake Avoided:**
Did not use a bar chart for this time-series view. A bar chart would technically work but loses the visual sense of momentum and trajectory. When comparing month-to-month movement, the connecting line between points is the information — it tells you rate of change. Bars just show magnitude at a single point.

Also did not start the Y-axis at a non-zero value. Even though all values are between 6M and 11M, the axis starts at 0M. Starting at, say, 5M would make normal monthly fluctuation look like dramatic swings — a classic misleading chart choice.

---

## Chart 3: Regional Performance View — Horizontal Bar Chart

**Business Question Answered:** Which regions are generating the most sales? How does the North/South/East/West split look?

**Why This Chart Type:**
A horizontal bar chart is the right choice when comparing a small number of named categories (four regions) on a single continuous measure (sales). The horizontal orientation makes region names easy to read without rotating text. The bars encode sales value in their length, which is the most perceptually accurate visual encoding available — the brain is very good at comparing horizontal lengths.

**Field Mapping:**
- Y-axis (rows): Region — East, North, South, West
- X-axis (columns): SUM([Sales])
- Labels: Values shown at the end of each bar (49M, 55M, 65M, 49M)
- Color: Single consistent blue tone across all bars (no color differentiation needed since region names already distinguish the bars)

**Data Story from This Chart:**
South leads at 65M, followed by North at 55M, with East and West tied at 49M. The visual makes this ranking immediately obvious without reading all four values.

**Design Principle Applied:**
Sorted bars by value (South at top, then North, then East/West). Sorting by value rather than alphabetically means the best performer is immediately visible and ranking is encoded visually in position. Alphabetical sorting (East, North, South, West) would require the reader to mentally re-rank.

**Mistake Avoided:**
Did not use a pie chart for regional comparison. Pie charts are very poor for comparing four similarly-sized segments — the human eye struggles to judge relative arc areas accurately, especially when values are close (49M, 49M, 55M, 65M). The bar chart makes the 65M vs 49M difference immediately clear; a pie slice would not.

Also considered a filled map (choropleth) but the dataset has four named regions rather than state-level data visible in this chart — a map would add geographic context without adding data clarity, so the bar chart was the cleaner choice.

---

## Chart 4: Category Profitability View — Horizontal Bar Chart (Grouped by Category and Sub-Category)

**Business Question Answered:** Which categories and sub-categories are generating the most sales? Where is the revenue concentrated within the product catalog?

**Why This Chart Type:**
A horizontal bar chart with a two-level hierarchy (Category → Sub-Category) on the rows axis is the most readable format for this kind of nested categorical comparison. The length of each bar encodes the sales value, and the grouping by category (Furniture, Office Supplies, Technology) allows the eye to compare both within-category sub-category performance and cross-category totals simultaneously.

**Field Mapping:**
- Y-axis (rows): Category (Furniture, Office Supplies, Technology) → Sub-Category nested underneath
- X-axis (columns): SUM([Sales]), scale from 0M to 45M
- Labels: Data labels shown on bars where values are large enough to display (12M for Bookcases, 13M for Furnishings, 2M for Art/Labels/Storage, 41M for Copiers, 38M for Phones)
- Color: Category-coded — Furniture sub-categories in one shade, Office Supplies in another, Technology in another (consistent with segment colors across the dashboard)

**Data Story from This Chart:**
Technology sub-categories completely dominate. Copiers (41M) and Phones (38M) are the two largest bars. Furniture sub-categories cluster in the 10–15M range. Office Supplies sub-categories are barely visible at 2M each. This is the most information-dense chart in the dashboard.

**Design Principle Applied:**
The two-level row hierarchy (Category + Sub-Category) is essential here. Showing sub-categories without their parent category grouping would make the chart harder to interpret — you'd see 17 bars in no particular order. The grouping adds structure and allows both granular and summary-level reading.

**Mistake Avoided:**
Did not use a treemap. Treemaps are visually striking but have two problems for this use case: they're very bad at representing small values (the tiny Office Supplies boxes would be nearly invisible), and they make precise comparison of similarly-sized areas difficult. The bar chart shows the 2M vs 41M difference far more clearly than any area-based chart would.

Did not use a stacked bar chart either — stacking would have obscured individual sub-category values within each category, which is exactly the detail leadership needs to see.

---

## Chart 5: Customer Segment View — Vertical Bar Chart

**Business Question Answered:** How do the three customer segments — Consumer, Corporate, and Home Office — compare in total sales? Which segment is generating the most revenue?

**Why This Chart Type:**
A vertical (column) bar chart is appropriate here because there are only three discrete categories (Consumer, Corporate, Home Office) being compared on a single measure (Sales). With only three items, the vertical format works well — the bars aren't too numerous to compare, and the vertical orientation gives a natural height-based comparison that the eye reads quickly. This also creates visual variety from the horizontal bar charts used in the regional and category views, making the dashboard less monotonous to look at.

**Field Mapping:**
- X-axis: Customer Segment (Consumer, Corporate, Home Office)
- Y-axis: SUM([Sales]), ranging from 0M to 80M
- Labels: Data labels above each bar (72M for Consumer, 71M for Corporate, 75M for Home Office)
- Color: Single consistent color tone across all three bars — differentiation comes from the x-axis labels, not color

**Data Story from This Chart:**
The three segments are remarkably close in sales: Consumer 72M, Corporate 71M, Home Office 75M. Home Office leads by a small margin. The visual makes the near-parity obvious — all three bars are almost the same height, with Home Office slightly taller.

**Design Principle Applied:**
Data labels above each bar are critical here because the differences between segments are small (4M spread across 71–75M values). Without labels, the visual difference in bar height would be nearly imperceptible. The labels let leadership read the exact numbers without squinting at the Y-axis.

Y-axis runs from 0M to 80M — slightly above the maximum value of 75M — giving the bars room to breathe visually without clipping at the top.

**Mistake Avoided:**
Did not use a pie chart. With three segments at 72M, 71M, and 75M, a pie chart would show three almost identical slices. The human eye cannot reliably distinguish between 33%, 33%, and 34% slices. The bar chart makes the near-equal distribution — and the slight Home Office lead — far more readable.

Did not add unnecessary color variation between the three bars. Some dashboard designers color each bar differently for segments, but when a single measure is being compared across categories, using the same color for all bars is cleaner and avoids implying that color carries additional meaning it doesn't have.

---

## Color and Layout Design Decisions

**Color Palette:** The dashboard uses a consistent corporate blue tone (similar to Tableau's default Steel Blue) across all bar charts. This was a deliberate choice — using the same color for bars in regional, category, and segment views means color doesn't carry conflicting meaning across charts. Where category differentiation was needed (in the Category Profitability view), text-based grouping via the row hierarchy was used instead of color.

**Layout Structure:**
- KPI Card anchored top-left — first thing the eye goes to
- Sales Trend View centered top — the most important trend, given premium placement
- Regional Performance View top-right — geographic context alongside the trend
- Category Profitability View bottom-left — large chart for a data-rich view
- Customer Segment View bottom-right — completes the bottom row

This layout follows a natural Z-pattern reading path (top-left → top-right → bottom-left → bottom-right) that matches how Western audiences scan a page, ensuring the most important information is encountered in the right sequence.

**Font and Label Consistency:** All chart titles use the same font size and weight. Axis labels are consistent in format (M suffix for millions, % suffix for rates). Sub-category labels in the Category view use a slightly smaller font to reflect their subordinate hierarchy — a subtle but effective way to reinforce the Category > Sub-Category relationship visually.

---

*All chart justifications are grounded in established data visualization principles: perceptual accuracy of visual encodings, appropriate match between data type and chart type, and clarity for a non-technical executive audience.*
