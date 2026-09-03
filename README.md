# CodeAlpha_DataVisualization

## Task 3: Data Visualization — Data Analytics Internship (CodeAlpha)

Portfolio-quality visuals built on the book price/rating dataset from
Tasks 1-2, using **Python, Matplotlib, and Seaborn**.

## Files

```
CodeAlpha_DataVisualization/
├── data_visualization.py         # Main script (run this)
├── product_price_analysis.csv    # Dataset (from Task 1/2)
├── visuals/
│   ├── 00_dashboard.png          # Combined 4-panel dashboard
│   ├── 01_catalog_composition.png
│   ├── 02_rating_vs_price.png
│   ├── 03_stock_by_tier.png
│   └── 04_top_value_items.png
└── requirements.txt
```

## Design approach

Every chart here is built to answer one specific business question and
support a decision — not just describe a column of numbers:

| Chart | Question it answers | Decision it supports |
|---|---|---|
| Catalog composition | Where does our inventory sit on price? | Assortment planning |
| Rating vs. price | Does higher rating justify higher price? | Pricing strategy |
| Stock by tier | Are any price tiers under-stocked? | Restocking priorities |
| Top value items | Which items over-deliver for their price? | Merchandising/featuring |

Design choices used throughout:
- **Consistent color palette** tied to price tier across every chart, so
  the same color always means the same thing
- **Direct annotations** (value labels on bars) instead of forcing the
  reader to estimate from gridlines
- **A one-line insight caption** under each chart — the chart shows the
  data, the caption states what to *do* with it
- **A combined dashboard** as the single "one-look" portfolio piece

## How to run

```bash
pip install -r requirements.txt
python data_visualization.py
```

Outputs 5 PNG files to `visuals/`.

## Key insights from this dataset

- The catalog skews heavily mid-range (75% of items) — a gap worth
  testing a broader premium assortment against.
- Rating and price are positively but only moderately correlated
  (r ≈ 0.39) — price isn't purely a proxy for quality here.
- Budget and mid-range tiers have the larger stock-availability gap
  (16-17% out of stock), not premium — restocking effort is better
  aimed there.
- A clear top-10 "best value" list highlights specific items worth
  featuring or using as loss-leaders.

## Notes

Completed as part of the CodeAlpha Data Analytics Internship —
Task 3: Data Visualization.
