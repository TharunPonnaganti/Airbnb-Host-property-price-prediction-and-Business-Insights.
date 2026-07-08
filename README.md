# Airbnb Host Property Price Prediction & Business Insights

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.7+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="scikit-learn">
  <img src="https://img.shields.io/badge/XGBoost-Boosting-006600?style=for-the-badge" alt="XGBoost">
  <img src="https://img.shields.io/badge/Pandas-Data-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
  <img src="https://img.shields.io/badge/Statsmodels-Statistics-4B8BBE?style=for-the-badge" alt="Statsmodels">
</p>

> **End-to-end machine learning pipeline** that analyzes 285,619 Airbnb listings across New York City's five boroughs to predict optimal listing prices. Built to power internal pricing tools that help hosts maximize profitability while keeping prices affordable for guests.

---

## Key Results

| Model | Train R² | Test R² | Test RMSE |
|:------|:--------:|:-------:|:---------:|
| Linear Regression | 0.637 | 0.635 | 27.06 |
| Ridge Regression | 0.637 | 0.635 | 27.06 |
| Lasso Regression | 0.637 | 0.635 | 27.07 |
| Elastic Net | 0.632 | 0.630 | 27.27 |
| Gradient Boosting | 0.755 | 0.727 | 25.98 |
| XGBoost | — | 0.692 | 24.91 |
| **Random Forest** | **0.976** | **0.848** | **—** |

**Best model: Random Forest** with R² = 0.848 on test data, capturing 85% of price variance.

---

## Business Insights

### Borough-Level Pricing

| Borough | Avg. Private Room | Avg. Entire Home | Avg. Hotel Room |
|:--------|:-----------------:|:-----------------:|:---------------:|
| Manhattan | Highest tier | Premium | Most expensive |
| Brooklyn | Mid-range | Mid-high | High |
| Queens | $67.93 | Mid-range | — |
| Bronx | $62.68 | Budget | — |
| Staten Island | $59.73 | Lowest | — |

### Host Tenure by Borough

| Borough | Avg. Years Active |
|:--------|:-----------------:|
| Brooklyn | 4.42 |
| Manhattan | 3.93 |
| Staten Island | 3.27 |
| Queens | 3.00 |
| Bronx | 2.94 |

> Brooklyn and Manhattan have the most established host communities, averaging ~4 years.

### Statistical Findings

| Hypothesis Test | Statistic | p-value | Conclusion |
|:----------------|:---------:|:-------:|:-----------|
| **Shapiro-Wilk** (price normality) | 0.159 | 0.0 | Price is **not** normally distributed |
| **Levene's Test** (price variance across room types) | 70.81 | 9.56e-46 | Significant variance differences across room types |
| **ANOVA** (price ~ borough) | F = 547.68 | < 0.001 | Borough significantly affects price |
| **ANOVA** (price ~ review score) | F = 218.67 | 1.95e-49 | Reviews significantly affect price |
| **ANOVA** (price ~ cancellation policy) | F = 72.02 | 5.48e-32 | Cancellation policy affects price |
| **ANOVA** (price ~ availability) | F = 633.26 | 2.23e-139 | Year-round availability strongly affects price |
| **Chi-Squared** (room type vs. borough) | 5016.09 | 0.0 | Strong association between room type and borough |

### Key Takeaways

- **Superhost status does not significantly affect pricing** — hosts can charge similar rates regardless.
- **Longer-tenured hosts tend to price lower**, suggesting competitive adjustment over time.
- **Hotel rooms in Manhattan/Brooklyn command premiums** above entire-home listings.
- **Cancellation policy and review scores** are statistically significant pricing factors.
- **148 amenity types** were engineered down to 28 grouped features, then split into common vs. special amenities.

---

## Pipeline Architecture

```
Raw Data (106 features, 285K rows)
│
├── 1. Data Processing
│   ├── Remove duplicates & irrelevant columns (106 → 38 features)
│   ├── Clean price/fee fields (strip $, commas → float)
│   ├── Parse & group 148 amenity types → 28 categories
│   └── KNN imputation for missing values
│
├── 2. Feature Engineering
│   ├── host_days_active_years (from host_since)
│   ├── avg_price_property_type (neighbourhood × property type)
│   ├── avg_review_score (neighbourhood × property type)
│   ├── special_amenities & common_amenities (aggregate counts)
│   ├── Outlier capping (IQR-based Winsorization)
│   └── Box-Cox transformation for skewed features
│
├── 3. Statistical Analysis
│   ├── Shapiro-Wilk, Levene's, ANOVA, Chi-Squared tests
│   ├── Autocorrelation, normality of residuals
│   ├── Homoscedasticity (Goldfeld-Quandt, Breusch-Pagan)
│   └── VIF-based multicollinearity removal
│
├── 4. Feature Selection
│   ├── RFE (Recursive Feature Elimination)
│   └── LassoCV regularization path
│
└── 5. Model Building
    ├── Linear / Ridge / Lasso / Elastic Net
    ├── Gradient Boosting (GridSearchCV tuned)
    ├── XGBoost
    └── Random Forest (GridSearchCV tuned) ← Best
```

---

## Repository Structure

```
├── airbnb-host-property-price-pred.ipynb   # Full analysis notebook
├── airbnbmark1.csv                         # Dataset (Git LFS, ~558 MB)
├── .gitattributes                          # LFS tracking config
└── README.md
```

## Dataset

| | |
|---|---|
| **Source** | [Kaggle — Airbnb NYC with 106 Features](https://www.kaggle.com/datasets/tharunponnaganti/airbnb-new-york-city-with-106-features) |
| **Records** | 285,619 listings |
| **Features** | 106 columns |
| **Size** | ~558 MB (CSV) |
| **Coverage** | All 5 NYC boroughs: Manhattan, Brooklyn, Queens, Bronx, Staten Island |

### Feature Categories

| Category | Examples |
|:---------|:---------|
| **Listing** | id, name, description, property_type, room_type, amenities |
| **Host** | host_since, host_response_time/rate, host_is_superhost, host_listings_count |
| **Location** | neighbourhood, borough, latitude, longitude, zipcode |
| **Property** | accommodates, bathrooms, bedrooms, beds, bed_type, square_feet |
| **Pricing** | price, weekly/monthly_price, security_deposit, cleaning_fee |
| **Availability** | availability_30/60/90/365, minimum/maximum_nights |
| **Reviews** | number_of_reviews, review_scores (rating, accuracy, cleanliness, checkin, communication, location, value) |
| **Policy** | cancellation_policy, instant_bookable |

## Getting Started

### Prerequisites

```bash
pip install numpy pandas matplotlib seaborn scikit-learn xgboost statsmodels scipy
```

### Clone & Setup

```bash
git clone https://github.com/TharunPonnaganti/Airbnb-Host-property-price-prediction-and-Business-Insights..git
cd Airbnb-Host-property-price-prediction-and-Business-Insights.
git lfs install
git lfs pull
```

### Run

Open `airbnb-host-property-price-pred.ipynb` in Jupyter Notebook or JupyterLab and run all cells.

---

## Tech Stack

| Purpose | Libraries |
|:--------|:----------|
| Data manipulation | NumPy, Pandas |
| Visualization | Matplotlib, Seaborn |
| Statistical testing | SciPy, Statsmodels |
| Machine learning | scikit-learn, XGBoost |
| Imputation | KNNImputer (sklearn) |
| Feature selection | RFE, LassoCV |

## License

This project uses data sourced from [Inside Airbnb](http://insideairbnb.com/). See the [Kaggle dataset page](https://www.kaggle.com/datasets/tharunponnaganti/airbnb-new-york-city-with-106-features) for licensing details.
