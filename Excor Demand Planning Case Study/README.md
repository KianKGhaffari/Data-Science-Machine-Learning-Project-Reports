# Excor Technologies — IBP Demand Planning Data Pipeline

**Case Study | Data Cleaning · Densification · Outlier Handling**

---

## Overview

This case study explores a data preparation pipeline built on a simulated Excor Technologies demand planning dataset. The goal is to transform raw monthly order data into a clean, complete, and analysis-ready format suitable for IBP forecasting and S&OP analytics.

The pipeline covers three stages: data cleaning, densification, and outlier handling. Each decision is documented with explicit reasoning to reflect how a data scientist would approach messy, real-world planning data in an enterprise environment.

---

## Business Context

Integrated Business Planning (IBP) unifies demand forecasting, supply planning, finance, and sales into a single aligned process. For IBP to function reliably, the underlying data must be complete, consistent, and free of structural errors. A missing month in a time series, a duplicate order record, or an undetected demand spike can propagate through every downstream forecast, safety stock calculation, and KPI.

This pipeline addresses those challenges systematically — surfacing each issue explicitly, handling it with documented reasoning, and producing a final dataset that a demand planner can trust.

---

## Dataset Overview

| Property | Detail |
|---|---|
| Source | Simulated Excor Technologies order data |
| Format | Excel (.xlsx), converted to CSV at ingestion |
| Rows | 1,808 |
| Date range | September 2020 — July 2026 |
| Grain | Monthly orders at model number level |

### Columns

| Column | Type | Description |
|---|---|---|
| `product_line` | string | High-level product category |
| `product_family` | string | Planning-level product grouping |
| `model_num` | string | Individual product model identifier |
| `region` | string | Geographic region — Americas, EMEA, APAC, AFO |
| `plant` | string | Manufacturing or fulfillment plant code |
| `order_type` | string | Transaction type — Trade, ZC1, ZSD1, ZSD2, ZGIO |
| `month_date` | datetime | First day of the order month |
| `qty_actual` | float | Units ordered |
| `average_selling_price_m` | float | Average selling price in millions USD |
| `total_est_actual_trade_values_m` | float | Estimated total revenue in millions USD |

---

## Pipeline Steps

1. **Data Cleaning** — standardizes column names, fixes type inconsistencies, flags duplicates, handles missing values, and removes logically invalid records

2. **Densification** — expands the sparse dataset into a complete monthly grid across every product family × region × order type combination, making zero-demand months explicit

3. **Outlier Handling** — detects extreme values using IQR per product family × region group and applies Winsorization to preserve time series continuity

---

## How to Run

**Requirements**

```bash
pip install pandas numpy matplotlib openpyxl
```

**Run**

```bash
python Excor_pipeline.py
```

The script loads the source file, executes all three steps with printed progress at each stage, and saves all outputs to the working directory.

---

## Outputs

| File | Description |
|---|---|
| `Excor_demand_clean_final.xlsx` | Final cleaned and densified dataset |
| `negative_qty_audit.xlsx` | Rows with negative quantity removed from demand dataset |
| `outlier_capping_audit.xlsx` | Records where qty_actual was capped, with original values preserved |
| `outliers_before.png` | Outlier detection plot per product family × region |
| `outliers_after.png` | Before vs after comparison per product family × region |

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.x | Core language |
| pandas | Data manipulation and densification |
| numpy | Numerical operations |
| matplotlib | Outlier visualizations |
| openpyxl | Excel file reading and writing |

---
