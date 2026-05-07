# 📊 Website Traffic & Revenue Analysis

> **End-to-end data analytics project** uncovering what actually drives revenue — not just traffic.  
> Tools: `Python` · `pandas` · `Power BI` · `DAX` &nbsp;|&nbsp; Dataset: 1,274 records · Jan–Jun 2024

---

## 📁 Table of Contents

- [Background & Overview](#-background--overview)
- [Data Structure Overview](#-data-structure-overview)
- [Executive Summary](#-executive-summary)
- [Insights Deep Dive](#-insights-deep-dive)
- [Recommendations](#-recommendations)
- [Project Structure](#-project-structure)
- [How to Run](#-how-to-run)

---

## 🔍 Background & Overview

### Business Problem

A digital business wanted to understand whether its marketing spend was generating proportional revenue  or simply driving traffic that doesn't convert. The core question:

> *"Are we investing in the right channels or are we optimising for vanity metrics?"*

### Objectives

- Identify which traffic sources deliver the highest revenue and conversion rates
- Determine whether session volume is a reliable proxy for revenue performance
- Uncover device-level and temporal patterns that affect conversion quality
- Translate findings into prioritised, actionable business recommendations

### Methodology

| Step | Description | Tool |
|------|-------------|------|
| 1. Problem definition | Framed the business question before touching the data | — |
| 2. Data cleaning & audit | Validated 1,274 rows, 19 columns — nulls, outliers, derived fields | `pandas` |
| 3. Correlation analysis | Pearson matrix across Sessions, Revenue, CVR, Bounce Rate | `pandas` · `seaborn` |
| 4. Segmentation | Grouped by Source, Device, Month, Weekday, Page, Campaign | `pandas` · `pivot_table` |
| 5. Cross-dimensional analysis | Traffic Source × Device revenue matrix | `pandas` · `unstack` |
| 6. Visualisation | Interactive dashboard with slicers for non-technical stakeholders | `Power BI` · `DAX` |

---

## 🗂 Data Structure Overview

### Dataset at a Glance

| Attribute | Type | Description |
|-----------|------|-------------|
| `Date` | Date | Daily record (Jan–Jun 2024) |
| `Traffic_Source` | Categorical | 7 channels: Email, Google Ads, Facebook, Instagram, Organic, Direct, Referral |
| `Device_Type` | Categorical | Mobile, Tablet, Desktop |
| `Country` | Categorical | Multi-country geographic origin |
| `Page_Visited` | Categorical | 6 types: Promo, Product, Blog, Checkout, Home, Landing |
| `Sessions` | Integer | Total visits per record |
| `Users` / `New_Users` | Integer | Unique and first-time visitors |
| `Bounce_Rate` | Float (%) | Single-page session percentage |
| `Avg_Session_Duration` | Integer (s) | Mean time on site in seconds |
| `Conversions` | Integer | Goal completions (purchases/sign-ups) |
| `Revenue` | Integer ($) | Revenue attributed to the record |
| `Campaign_Name` | Categorical | 7 campaign types including No Campaign |
| `Month` / `Day` / `Weekday` | Temporal | Derived date parts for time-series analysis |
| `Conversion_Rate` *(derived)* | Float (%) | `Conversions / Sessions` |
| `Revenue_per_Session` *(derived)* | Float ($) | `Revenue / Sessions` |

### Data Quality

- ✅ No null values across all 19 columns
- ✅ Derived fields validated against source columns
- ✅ No duplicate date-source-device-page combinations
- ⚠️ `Conversion_Rate` max: 39% — flagged as a spike, retained as plausible outlier
- ℹ️ `Revenue_per_Session` used as the primary diagnostic metric (normalises for session volume differences)

---

## 📋 Executive Summary

### The Central Finding

> **Sessions and Revenue have a Pearson correlation of r = 0.012 — effectively zero.**

A business optimising for session volume is not, in this dataset, optimising for revenue. `Conversion_Rate`, by contrast, correlates with Revenue at **r = 0.51**. Traffic *quality* matters far more than traffic *quantity*.

### Key Metrics at a Glance

| Metric | Value |
|--------|-------|
| Total Revenue | **$941,205** |
| Total Sessions | **363,116** |
| Average Conversion Rate | **8.5%** |
| Sessions ↔ Revenue correlation | **r = 0.012** |
| CVR ↔ Revenue correlation | **r = 0.51** |
| Peak month | **March** ($173,832 · +14.8% vs avg) |
| Top channel by revenue | **Email Marketing** ($141,302) |
| Top channel by CVR | **Direct** (9.13%) |

### Performance Summary by Dimension

| Dimension | Top Performer | Bottom Performer | Gap |
|-----------|---------------|------------------|-----|
| Traffic Source (Revenue) | Email Marketing · $141K | Google Ads · $127K | $14.6K |
| Traffic Source (CVR) | Direct · 9.13% | Google Ads · 7.50% | 1.63 pp |
| Device (Revenue) | Mobile · $340K | Desktop · $300K | $40K |
| Device (CVR) | Tablet · 8.84% | Desktop · 8.20% | 0.64 pp |
| Month | March · $173.8K | April · $144.7K | $29.1K |
| Page (Revenue) | Promo Page · $182K | Landing Page · $124K | $58K |

---

## 🔬 Insights Deep Dive

### 1. Traffic Source Analysis

| Traffic Source | Revenue | Sessions | CVR | Rev/Session |
|----------------|---------|----------|-----|-------------|
| Email Marketing | $141,302 | 54,241 | 8.28% | $2.61 |
| Instagram Ads | $139,978 | 51,516 | 8.84% | $2.72 |
| Facebook Ads | $136,990 | 53,976 | 7.95% | $2.54 |
| Referral | $135,919 | 51,342 | 8.57% | $2.65 |
| Organic Search | $132,002 | 49,400 | 9.12% | $2.67 |
| Direct | $128,327 | 49,086 | 9.13% | $2.61 |
| Google Ads | $126,687 | 53,555 | 7.50% | $2.37 |

**Key findings:**
- **Email Marketing** generates the highest gross revenue ($141K) from a warm, high-intent audience
- **Organic Search & Direct** achieve the highest CVRs (9.12–9.13%) with zero paid acquisition cost
- **Google Ads** drives the most sessions (53,555) but the lowest CVR (7.50%) and Rev/Session ($2.37) — the clearest case of volume over quality in the dataset
- **Instagram Ads** shows the strongest CVR among paid channels (8.84%), suggesting targeting or creative quality advantages

---

### 2. Device Performance

| Device | Revenue | Sessions | CVR | Rev/Session |
|--------|---------|----------|-----|-------------|
| Mobile | $339,551 | 134,995 | 8.41% | $2.52 |
| Tablet | $301,440 | 115,132 | 8.84% | $2.62 |
| Desktop | $300,214 | 112,989 | 8.20% | $2.66 |

**Key findings:**
- **Mobile** dominates session volume (53% of all sessions) and total revenue, but converts 0.43 pp below Tablet
- **Tablet** users convert at the highest rate (8.84%) — implying more intentional, lower-friction browsing
- **Desktop** delivers the highest Rev/Session ($2.66) despite the lowest CVR, possibly reflecting higher average order values
- The Mobile CVR gap is the clearest UX optimisation opportunity in the dataset

---

### 3. Temporal Patterns

**Monthly Revenue:**

| Month | Revenue | Sessions | vs. Period Avg |
|-------|---------|----------|----------------|
| March | $173,832 | 64,279 | +14.8% ↑ |
| January | $158,412 | 61,942 | +4.8% ↑ |
| May | $157,972 | 61,341 | +4.5% ↑ |
| February | $153,978 | 56,655 | +1.9% ↑ |
| June | $152,345 | 58,312 | +0.7% ↑ |
| April | $144,666 | 60,587 | -4.5% ↓ |

**Day-of-week:** Friday and Sunday are peak revenue days. Wednesday is consistently the weakest.

**Key findings:**
- March outperforms the six-month average by 14.8%, driven by the Winter Sale campaign
- The April dip follows March's peak — a likely post-campaign demand pull-forward effect
- May had nearly identical sessions to March but $16K less revenue, further confirming that volume ≠ revenue

---

### 4. Page-Level Performance

| Page | Revenue | Sessions | CVR |
|------|---------|----------|-----|
| Promo Page | $182,099 | 68,402 | 8.64% |
| Product Page | $174,751 | 65,756 | 8.80% |
| Blog Page | $161,629 | 63,908 | 8.07% |
| Checkout Page | $153,918 | 59,927 | 8.22% |
| Home Page | $145,120 | 55,913 | 8.55% |
| Landing Page | $123,688 | 49,210 | 8.63% |

**Key findings:**
- **Promo & Product pages** lead on both revenue and CVR — high purchase intent entry points
- **Blog Page** earns $161.6K despite the lowest CVR (8.07%) purely due to session volume — a volume problem, not a conversion problem
- **Landing Page** has a reasonable CVR (8.63%) but the lowest revenue — a session acquisition problem, not a conversion problem (different root cause, different fix)

---

### 5. Correlation Analysis

| Metric Pair | r | Interpretation |
|-------------|---|----------------|
| Conversion Rate ↔ Revenue | **+0.51** | Moderate positive — quality drives revenue |
| Rev/Session ↔ Revenue | **+0.67** | Strong positive — best revenue predictor |
| Sessions ↔ Revenue | **+0.01** | No meaningful relationship |
| Sessions ↔ Conversion Rate | **-0.64** | Negative — volume dilutes quality |
| Bounce Rate ↔ Revenue | **+0.004** | No relationship |

> 💡 The **negative correlation between Sessions and CVR (r = −0.64)** is the most counter-intuitive finding: when volume goes up, conversion quality tends to go down — a classic paid acquisition dilution effect.

---

## 💡 Recommendations

### 1. Reallocate budget from Google Ads → Email & Organic
Google Ads drives the most sessions but the weakest Rev/Session ($2.37 vs $2.67 for Organic). Shifting 20% of paid budget to email nurture and SEO content would improve ROAS within 90 days.  
**Estimated impact:** +8–12% revenue efficiency on current traffic

### 2. Fix the mobile conversion gap
Closing half the Mobile-to-Tablet CVR gap (0.43 pp) at current session volumes yields ~290 additional conversions.  
**Recommended tests:** checkout form layout, CTA size, load speed, Apple/Google Pay integration  
**Estimated impact:** $12,000–$18,000 incremental annual revenue

### 3. Schedule campaigns around March & Fri/Sun peaks
March outperforms the average by 14.8%. Friday and Sunday are the top revenue days. Aligning budget peaks and email send times to these windows amplifies returns at no additional cost.  
**Estimated impact:** 15–20% lift in seasonal campaign ROI

### 4. Add conversion pathways to Blog & Home pages
Blog Page CVR (8.07%) is the lowest of all pages despite high traffic. Home Page has the fewest sessions for a default entry point.  
**Actions:** contextual product modules in blog posts, exit-intent overlays, A/B test Home page CTA placement  
**Estimated impact:** 5–10% lift in overall on-site CVR

### 5. Replace session KPIs with conversion-quality metrics
Sessions as a primary KPI actively works against revenue (r = −0.64 with CVR). Replace with **Revenue-per-Session** and **CVR by source** in marketing dashboards.  
**Strategic impact:** every planning cycle optimises for value, not volume

---

## 📁 Project Structure

```
website-traffic-analysis/
│
├── data/
│   ├── raw/                         # Original dataset
│   └── cleaned_website_traffic.csv  # Cleaned, analysis-ready data
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb       # Null checks, type validation, derived fields
│   ├── 02_eda.ipynb                 # Distributions, group stats, outlier review
│   ├── 03_correlation_analysis.ipynb# Pearson matrix, heatmap
│   └── 04_segmentation.ipynb        # Source, device, page, temporal breakdowns
│
├── dashboard/
│   └── traffic_revenue_dashboard.pbix  # Power BI file
│
├── visuals/
│   └── *.png                        # Exported charts for README and portfolio
│
└── README.md
```

---

## ⚙️ How to Run

**1. Clone the repo**
```bash
git clone https://github.com/PatienceAnono/website-traffic-analysis.git
cd website-traffic-analysis
```

**2. Install dependencies**
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

**3. Run notebooks in order**
```bash
jupyter notebook notebooks/01_data_cleaning.ipynb
```

**4. Open the Power BI dashboard**

Open `dashboard/traffic_revenue_dashboard.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/).

---

## 🔖 Analytical Limitations

- Dataset covers H1 2024 only — seasonality beyond June is unconfirmed
- Campaign cost data is absent, making true ROI/ROAS calculation impossible
- Attribution is last-touch by default — multi-touch modelling would refine channel credit
- Country-level segmentation not explored in depth — geographic CVR differences may exist

---

*Patience Anono · Data Analytics Portfolio · [github.com/PatienceAnono/website-traffic-analysis](https://github.com/PatienceAnono/website-traffic-analysis)*





