# Student Bag Market Analysis

## Overview
A complete data pre-processing and exploratory data analysis (EDA) project investigating one specific hypothesis: do functional features: waterproof material and laptop compartments, the two most critical attributes for students actually drive higher bag prices?

## Problem Statement
**Hypothesis:** Are the Waterproof and Laptop Compartment features reflected in bag price? Or is price influenced by other, unmeasured attributes?

This question matters commercially: if functional features carry no price premium, they may have already become a market standard rather than a differentiator, a useful insight for product positioning.

## Dataset
- **Source:** Kaggle
- **Size:** 12,805 rows × 10 columns
- **Columns:** Brand, Material, Size, Compartments, Laptop Compartment, Waterproof, Style, Color, Weight Capacity (kg), Price
- **Note:** Price range ($15–$150), weight capacity (5–30kg), and compartments (1–10) were all suspiciously uniform, an early signal that this dataset is synthetic, confirmed later through distribution testing.

## Data Cleaning & Preparation
Missing values affected **~40% of rows** (about 5% per column) and were handled per-column based on data characteristics:

| Column(s) | Strategy | Reasoning |
|---|---|---|
| Brand, Color | Filled with `"Unknown"` | Unbranded/unlisted items are realistic in real markets |
| Material, Size, Style, Laptop Compartment, Waterproof | Filled with mode | Few, well-defined categories; preserves binary structure for encoding |
| Weight Capacity | Filled with median | Continuous variable, robust to outliers |
| Price | **Left as missing** | Imputing would distort the distribution; a separate `df_viz` dataset excluding Price nulls was used for all visualization and statistical work |

**Data type fixes:**
- `Compartments`: float → integer
- `Size`: converted to ordered Categorical (Small < Medium < Large)
- All other string columns → category dtype (memory efficiency)

**Outlier check:** IQR method applied to Price and Weight Capacity → **zero outliers found**, confirmed visually via boxplot.

## Feature Engineering
- Label-encoded `Laptop_Compartment` and `Waterproof` → 0/1
- Created `nilai_fungsional` (functional value score): sum of both encoded features → 0 (none), 1 (one), or 2 (both)
- Filtered for "ideal student bags" (both features present): **891 bags (7.3%)** of the dataset

## Statistical Analysis

| Test | Result | Interpretation |
|---|---|---|
| T-test (Waterproof vs Price) | p > 0.05 | No significant price difference |
| T-test (Laptop Compartment vs Price) | p > 0.05 | No significant price difference |
| Kolmogorov-Smirnov (Normality) | Deviation 0.0618 | Poor fit to normal distribution |
| Kolmogorov-Smirnov (Uniformity) | Deviation 0.0243 | **Strong fit to uniform distribution** confirms synthetic data |
| PMF | NF=0: 28%, NF=1: 50%, NF=2: 22% | Feature combinations evenly spread, prices nearly identical across groups |

## Key Findings
- Waterproof bags average **$81.46** vs non-waterproof **$82.01** a $0.55 difference
- Laptop compartment bags average **$81.30** vs without **$82.15**  a $0.85 difference
- **Neither feature has a statistically significant effect on price**
- Bags with both features are, counterintuitively, slightly *cheaper* than bags with none, suggesting these features are now standard, not premium
- Brand also showed no real pricing power: most expensive (Puma, $82.42) vs cheapest (Jansport, $81.50), only a $0.92 gap within a $15–$150 range
- Correlation heatmap confirmed: all numeric variables show near-zero correlation with Price (max ~0.01)

## Validation & Next Steps
A 10% random sample (1,280 rows) was tested against the full dataset, mean price difference was under 1%, confirming the uniform distribution allows reliable random sampling for future modeling (e.g. K-Means Clustering, Decision Tree).

**Recommendation:** This dataset needs enrichment with real sales volume, actual transaction prices, buyer reviews, and time-series data before being used for any pricing or inventory decision.

## Tools Used
Python · Pandas · NumPy · Matplotlib · Seaborn · Plotly · Scipy (T-test, KS-test) ·


*Part of the Data Analytics Bootcamp portfolio (2026)*
