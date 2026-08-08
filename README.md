# Real-Time Sales Dashboard — Data Analytics Internship

## About this project

This is my final project for the Data Analytics internship, built entirely in Google Looker
Studio. I worked with an e-commerce order dataset (order details, payment methods, and product/SKU
information) to build a set of dashboards that answer real business questions — the kind Sales,
Finance, and Marketing teams would actually ask.

I built this on top of the same dataset I used during my training project, and added six new
analytics tasks on top of it as part of the internship.

🔗 **Live Dashboard:** https://datastudio.google.com/reporting/5bd2ff5a-2c20-4b6d-bcdd-6fc9ae0de763

---

## What I used

- Google Looker Studio for building the dashboards
- Google Sheets for cleaning up the raw data before connecting it
- Blended data sources to join tables that didn't share a single common view
- Calculated fields (CASE, WEEKDAY, MONTH, CONCAT, COUNT_DISTINCT) for custom logic that
  Looker Studio doesn't handle out of the box

---

## The six dashboards

### 1. Monthly Revenue Trend (2022)
A line chart tracking total revenue (`SUM(before_discount)`) across every month of 2022.

Revenue peaked in **August (₹68.6 Cr)** and **September (₹65.8 Cr)**, with another strong
month in April (₹67.9 Cr). Then it fell off a cliff in Q4 — October, November and December
all came in under ₹15 Cr. That drop lines up with what I found later in the weekend/weekday
analysis too, so it looks like a genuine slowdown in the last quarter, not a data issue.

### 2. Sales by Payment Method
A table breaking down total sales and quantity by `payment_method`, built by blending
`order_detail` with `payment_detail` on `payment_id`.

**cod** (cash on delivery) is the clear leader — ₹28.97 Cr across 7,408 orders. **Payaxis**
came second at ₹23.63 Cr but from far fewer orders (1,633), meaning its average order value
is much higher than cod's. On the other end, `marketingexpense` and `productcredit` barely
register — under ₹6 lakh each.

### 3. Average Order Value (AOV) Trend
`SUM(after_discount) / COUNT_DISTINCT(order_id)`, grouped by month.

AOV stayed fairly flat for most of the year — somewhere between ₹9.5L and ₹18.5L a month.
Then August and September spike hard, to ₹53.6L and ₹58.8L. That's a 4-5x jump, which almost
certainly means a handful of very large orders came in during those two months rather than
a real shift in typical customer spend — and it matches the revenue spike in the same months
from the first dashboard.

### 4. Revenue vs. Discount Impact by Category
A grouped bar chart comparing `before_discount` and `after_discount` revenue per category,
with a calculated `Discount_Impact` field.

Mobiles & Tablets dominates by a wide margin (~₹3.5 Bn), with Computing and Appliances a
distant second and third (~₹1.2-1.3 Bn each). What stood out to me is that the before/after
discount bars are almost identical across every category — discounts aren't eating into
revenue much at all at this level of aggregation.

### 5. Products with the Biggest Sales Drop (2021 vs 2022)
Two separate yearly aggregations by `sku_name`, compared to find the top 10 products that
lost the most volume year over year.

One product stands well above the rest — a ~240 unit drop, nearly 50% more than the next
one down. The next few products cluster in the 130-165 unit range, and the tail end of the
top 10 is closer to 50 units. Worth flagging the top 2-3 to the Sales team for a closer look.

### 6. Weekend vs. Weekday Sales — Q4 2022
This one took the most debugging. The task asked for *average daily sales*, which isn't the
same as just averaging order values — I had to first sum sales per calendar day, and only
then average across days. I ended up self-blending `order_detail` on itself to get daily
totals, then added `Day_Type` and `Month` calculated fields on top using `WEEKDAY()` and
`MONTH()`.

Overall, weekdays outsell weekends (₹1.61 Cr/day vs ₹1.48 Cr/day). That holds for October and
especially November (₹1.89 Cr vs ₹1.52 Cr). But December flips — weekend sales (₹1.46 Cr)
edge past weekday (₹1.35 Cr), probably a holiday-shopping effect. So the short answer for the
Campaign team: weekend promos only really paid off in December.

---

## A few things I ran into

Looker Studio's blending is powerful but has some rough edges — join conditions get wiped
if you're not careful editing a blend, calculated fields built inside one chart don't carry
over to others unless they're defined at the data-source or blend level, and field names with
spaces break formula autocomplete. All fixable once you know what's happening, but it took a
few rounds of trial and error to get there.

---

## Data used

- `order_detail` — order-level data: order id, date, price, quantity, before/after discount, payment id
- `payment_detail` — maps payment_id to payment_method
- `sku_detail` — maps sku_id to product name and category



---

**Lucky Pal**
Data Analytics Intern
