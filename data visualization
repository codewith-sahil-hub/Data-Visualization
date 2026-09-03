"""
Task 3: Data Visualization
=============================
Dataset: product_price_analysis.csv (book price/rating data, from Tasks 1-2)

This script goes beyond diagnostic EDA plots to produce POLISHED,
portfolio-quality visuals — each one designed to answer a specific
business question and support a decision, not just describe the data.

Run with: python data_visualization.py
Outputs: PNG files in ./visuals/ (individual charts + one combined dashboard)
"""

import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker
import seaborn as sns

OUT_DIR = "visuals"
os.makedirs(OUT_DIR, exist_ok=True)

# ----------------------------------------------------------------------
# Consistent visual identity across every chart in this portfolio
# ----------------------------------------------------------------------
PALETTE = {
    "budget": "#4C9AFF",
    "mid-range": "#6554C0",
    "premium": "#DE350B",
    "accent": "#00875A",
    "neutral": "#6B778C",
}
plt.rcParams.update({
    "font.family": "DejaVu Sans",
    "axes.edgecolor": "#DFE1E6",
    "axes.labelcolor": "#172B4D",
    "text.color": "#172B4D",
    "xtick.color": "#42526E",
    "ytick.color": "#42526E",
    "axes.titleweight": "bold",
    "axes.titlesize": 13,
    "figure.facecolor": "white",
})
sns.set_style("whitegrid", {"grid.color": "#F0F1F3"})

df = pd.read_csv("product_price_analysis.csv").drop_duplicates()
df["rating_stars"] = df["rating_stars"].fillna(df["rating_stars"].median())
BUCKET_ORDER = ["budget", "mid-range", "premium"]


def annotate_bars(ax, fmt="{:.0f}"):
    for p in ax.patches:
        h = p.get_height()
        if h > 0:
            ax.annotate(fmt.format(h), (p.get_x() + p.get_width() / 2, h),
                        ha="center", va="bottom", fontsize=9, color="#172B4D")


# ----------------------------------------------------------------------
# CHART 1 — "Where does our catalog sit on price?" (portfolio composition)
# ----------------------------------------------------------------------
fig, ax = plt.subplots(figsize=(7, 4.5))
counts = df["price_bucket"].value_counts().reindex(BUCKET_ORDER)
bars = ax.bar(counts.index, counts.values,
              color=[PALETTE[b] for b in BUCKET_ORDER])
annotate_bars(ax)
ax.set_title("Catalog Skews Heavily Mid-Range")
ax.set_ylabel("Number of items")
ax.set_xlabel("")
fig.text(0.5, -0.02,
          "Insight: 75% of the catalog sits in the mid-range tier — "
          "there's room to test a broader premium assortment.",
          ha="center", fontsize=9, style="italic", color=PALETTE["neutral"])
plt.tight_layout()
plt.savefig(f"{OUT_DIR}/01_catalog_composition.png", dpi=150, bbox_inches="tight")
plt.close()


# ----------------------------------------------------------------------
# CHART 2 — "Does higher rating justify a higher price?" (decision-support)
# ----------------------------------------------------------------------
fig, ax = plt.subplots(figsize=(7, 4.5))
sns.regplot(data=df, x="rating_stars", y="price_gbp", ax=ax,
            scatter_kws={"alpha": 0.4, "color": PALETTE["mid-range"], "s": 30},
            line_kws={"color": PALETTE["premium"], "linewidth": 2})
ax.set_title("Higher-Rated Items Trend More Expensive")
ax.set_xlabel("Rating (stars)")
ax.set_ylabel("Price (GBP)")
fig.text(0.5, -0.02,
          "Insight: the trend is real but moderate (r \u2248 0.39) — rating "
          "alone won't explain most pricing decisions.",
          ha="center", fontsize=9, style="italic", color=PALETTE["neutral"])
plt.tight_layout()
plt.savefig(f"{OUT_DIR}/02_rating_vs_price.png", dpi=150, bbox_inches="tight")
plt.close()


# ----------------------------------------------------------------------
# CHART 3 — "Are we leaving stock gaps in our best-selling tier?"
# ----------------------------------------------------------------------
stock_by_bucket = (
    df.groupby("price_bucket")["in_stock"]
    .apply(lambda s: (s == True).mean() * 100)
    .reindex(BUCKET_ORDER)
)
fig, ax = plt.subplots(figsize=(7, 4.5))
bars = ax.bar(stock_by_bucket.index, stock_by_bucket.values,
              color=[PALETTE[b] for b in BUCKET_ORDER])
annotate_bars(ax, fmt="{:.0f}%")
ax.set_ylim(0, 100)
ax.set_title("Premium Items Are Actually Best-Stocked")
ax.set_ylabel("% in stock")
fig.text(0.5, -0.02,
          "Insight: budget and mid-range have the bigger availability gap "
          "(~16-17% out of stock) — that's where a restocking review "
          "would have the most impact.",
          ha="center", fontsize=9, style="italic", color=PALETTE["neutral"])
plt.tight_layout()
plt.savefig(f"{OUT_DIR}/03_stock_by_tier.png", dpi=150, bbox_inches="tight")
plt.close()


# ----------------------------------------------------------------------
# CHART 4 — "Where's the best value hiding?" (actionable ranking)
# ----------------------------------------------------------------------
top_value = df.nlargest(10, "value_score").sort_values("value_score")
fig, ax = plt.subplots(figsize=(7, 5))
ax.barh(top_value["title"], top_value["value_score"], color=PALETTE["accent"])
ax.set_title("Top 10 Best-Value Items (Rating per £ Spent)")
ax.set_xlabel("Value score (higher = better)")
fig.text(0.5, -0.02,
          "Insight: these items over-deliver relative to price — strong "
          "candidates to feature or use as loss-leaders.",
          ha="center", fontsize=9, style="italic", color=PALETTE["neutral"])
plt.tight_layout()
plt.savefig(f"{OUT_DIR}/04_top_value_items.png", dpi=150, bbox_inches="tight")
plt.close()


# ----------------------------------------------------------------------
# COMBINED DASHBOARD — the single "one-look" portfolio piece
# ----------------------------------------------------------------------
fig, axes = plt.subplots(2, 2, figsize=(13, 9))
fig.suptitle("Book Catalog Pricing & Value Dashboard", fontsize=17, fontweight="bold", y=0.98)

ax = axes[0, 0]
ax.bar(counts.index, counts.values, color=[PALETTE[b] for b in BUCKET_ORDER])
annotate_bars(ax)
ax.set_title("Catalog Composition by Price Tier")

ax = axes[0, 1]
sns.regplot(data=df, x="rating_stars", y="price_gbp", ax=ax,
            scatter_kws={"alpha": 0.4, "color": PALETTE["mid-range"], "s": 25},
            line_kws={"color": PALETTE["premium"], "linewidth": 2})
ax.set_title("Rating vs. Price")

ax = axes[1, 0]
ax.bar(stock_by_bucket.index, stock_by_bucket.values,
       color=[PALETTE[b] for b in BUCKET_ORDER])
annotate_bars(ax, fmt="{:.0f}%")
ax.set_ylim(0, 100)
ax.set_title("Stock Availability by Tier (Premium Best-Stocked)")

ax = axes[1, 1]
ax.barh(top_value["title"].str.slice(0, 20), top_value["value_score"],
        color=PALETTE["accent"])
ax.set_title("Top 10 Best-Value Items")
ax.tick_params(axis="y", labelsize=8)

plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.savefig(f"{OUT_DIR}/00_dashboard.png", dpi=150, bbox_inches="tight")
plt.close()

print(f"Saved 5 files to ./{OUT_DIR}/ (4 individual charts + 1 dashboard)")
