# Jumia Product Performance Dashboard

An interactive Excel dashboard analyzing pricing, discounts, and customer reviews for products listed on Jumia, built to surface insights that could support better pricing, promotion, and customer-engagement decisions.

## 1. Project Overview

**Objective:** Analyze a dataset of 112 Jumia products — pricing, discounts, reviews, and ratings — to identify trends and build an interactive dashboard that helps Jumia and its sellers understand what drives product performance.

**Dataset:** Raw product listings with the following original fields:
- `Product` — product name
- `Current Price` — selling price (KSh)
- `Old Price` — pre-discount price (KSh)
- `Discount` — discount percentage
- `Review` — number of customer reviews
- `Rating` — average customer rating (out of 5)

**Deliverable:** `jumia_product_performance_analysis.xlsx`, containing raw data, cleaned data, descriptive/trend analysis, category breakdowns, product performance tables, and a slicer-driven dashboard.

## 2. Process

### Step 1 — Data Cleaning (`Raw_Data` → `Cleaned_Data`)
- Stripped currency symbols and commas from `Current Price` and `Old Price` and converted both to numeric.
- Parsed `Rating` out of its original text format (e.g. "4.5 out of 5") into a numeric value.
- Corrected `Review` values that were stored as negative numbers.
- Checked for and handled missing values — **55 of 112 products (about 49%) had no rating recorded**, which were flagged as `"Not Provided"` rather than dropped, so they wouldn't silently skew averages.
- Checked for duplicate records and inconsistent entries.

### Step 2 — Data Enrichment
Added calculated columns to support analysis:
- **Discount Amount** = Old Price − Current Price
- **Rating Category** — Poor (<3), Average (3–4), Excellent (>4.5)
- **Discount Category** — Low (<20%), Medium (20–40%), High (>40%)
- **Price Category** — Low, Medium, High (based on price quartiles: Q1 ≈ KSh 493, Q2 ≈ KSh 1,670)

### Step 3 — Analysis
Performed on the `Data_Analysis`, `Product Categories`, `Product Performance`, and `Trend Analysis` sheets:
- Descriptive statistics (averages, totals, price extremes)
- Correlation analysis between discount/review, rating/review, and price/rating
- Top/bottom 5 and top 10 product rankings by rating, reviews, and discount
- Seller-performance flagging using rule-based logic (e.g. "high discount + low engagement")

### Step 4 — Dashboard
Built an interactive dashboard (`Dashboard` sheet) using PivotTables, PivotCharts, bar/column/line/pie charts, conditional formatting, and slicers for Rating Category, Discount Category, and Price Category.

## 3. Key Findings

**Descriptive statistics (n = 112 products):**

| Metric | Value |
|---|---|
| Total products | 112 |
| Total reviews | 723 |
| Average current price | KSh 1,186.89 |
| Average old price | KSh 1,811.11 |
| Average discount | 36.8% |
| Average rating (where provided) | 3.89 / 5 |
| Most expensive product | 32PCS Portable Cordless Drill Set (KSh 3,750) |
| Least expensive product | 3PCS Single Head Knitting Crochet Sweater Needle Set (KSh 38) |

**Correlations:**

| Relationship | Correlation | Interpretation |
|---|---|---|
| Discount % vs. Reviews | −0.14 | No — higher discounts do **not** drive more reviews |
| Rating vs. Reviews | +0.06 | Slight, essentially no relationship |
| Price vs. Rating | +0.11 | Slight — expensive products aren't meaningfully better rated |

**Category breakdown:**

| Rating | Count | | Discount | Count | | Price | Count |
|---|---|---|---|---|---|---|---|
| Excellent | 23 | | High (>40%) | 62 | | High | 88 |
| Average | 22 | | Medium (20–40%) | 32 | | Medium | 7 |
| Poor | 12 | | Low (<20%) | 18 | | Low | 17 |
| Not Provided | 55 | | | | | | |

**Seller-performance flags (rule-based):**

| Flag | Count | Rule |
|---|---|---|
| Strong customer engagement | 27 | Reviews ≥ 8 |
| High discount + low engagement | 50 | Discount > 40% AND Reviews < 8 |
| Many reviews + average rating | 14 | Reviews ≥ 8 AND Rating 3.0–4.5 |
| High discount + low rating | 10 | Discount > 40% AND Rating < 3 |

**Top performers:** Most-reviewed products included a 120W Cordless Vacuum Cleaner (69 reviews), a 137-piece cake decorating set (55), and a digital vernier caliper (49). Several perfect-rating (5.0) products — such as an anti-skid coaster and a peacock throw pillow — had only 1–3 reviews each, meaning top ratings were often based on very small sample sizes.

**Data quality note:** One product name ("DIY File Folder, Office Drawer File Holder...") appears in both the top-5 and bottom-5 rating tables at different prices, suggesting duplicate/near-duplicate listings under the same name — worth deduplicating in a production version of this dataset.

## 4. Business Insights

- **Discounting alone does not drive engagement.** The discount–review correlation is essentially zero to slightly negative, and 50 of 112 products (45%) combine a discount over 40% with fewer than 8 reviews. Deep discounts are not translating into customer interest for nearly half the catalog.
- **Popularity is not a proxy for quality.** The single most-reviewed product (120W Cordless Vacuum Cleaner, 69 reviews) has a "Poor" rating of 2.8/5 — high visibility with unresolved satisfaction issues.
- **Price doesn't buy trust.** The weak price–rating correlation (+0.11) shows customers don't necessarily rate expensive products higher; quality perception is driven by something other than price point.
- **Rating data is a major gap.** Nearly half the catalog (55 products) has no rating at all, which limits both buyer confidence and the reliability of any rating-based analysis.
- **High discount + low rating (10 products) signals a quality problem, not a pricing one** — these sellers are already discounting heavily and still underperforming, so further discounting is unlikely to help.

## 5. Recommendations

1. **Don't treat discounting as an engagement lever on its own.** For the 50 "high discount, low engagement" products, invest in better product photography, descriptions, and search placement rather than deeper markdowns.
2. **Prioritize review generation over discount depth.** With 49% of products lacking any rating, Jumia should nudge post-purchase review prompts or small incentives, especially for products with strong sales but few reviews.
3. **Flag high-review, low-rating products for quality review**, not further marketing — the vacuum cleaner case shows visibility without satisfaction is a retention risk.
4. **Treat the 10 "high discount + low rating" products as a product-quality escalation**, not a pricing problem — additional discounts are unlikely to fix an underlying satisfaction issue.
5. **Be cautious with perfect-rating products that have very few reviews** — a 5.0 rating on 1–2 reviews is not statistically meaningful and shouldn't be a primary merchandising signal until review counts grow.
6. **Clean up duplicate/near-duplicate listings** (e.g., same product name at different prices/ratings) before drawing further conclusions from the dataset.

## 6. Tools Used

- Microsoft Excel — data cleaning, formulas, PivotTables, PivotCharts, slicers, conditional formatting

## 7. Repository Structure

```
.
├── README.md
└── jumia_product_performance_analysis.xlsx
```

## 8. Note

This project was completed as a practice exercise analyzing a real-world-style Jumia product dataset, covering the full workflow from raw data cleaning through to an interactive, slicer-driven Excel dashboard.
