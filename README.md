## Cinema Audience Forecasting

Time-series forecasting of daily theatre audience counts across multiple locations using booking platform data.

## Problem Statement

Predict daily cinema audience attendance using historical data from two booking platforms:
- **BookNow**: Online ticket booking and aggregation platform
- **CinePOS**: Point-of-sale system for on-site ticket sales

## Dataset

The competition provides the following data files:

| File | Description |
|------|-------------|
| `booknow_visits.csv` | Historical daily audience counts |
| `booknow_booking.csv` | Online booking transactions |
| `cinePOS_booking.csv` | POS booking transactions |
| `booknow_theaters.csv` | Theatre metadata from BookNow |
| `cinePOS_theaters.csv` | Theatre metadata from CinePOS |
| `movie_theater_id_relation.csv` | Mapping between BookNow and CinePOS theatres |
| `date_info.csv` | Calendar information |
| `sample_submission.csv` | Submission format |

**Key Characteristics:**
- Time-series data spanning multiple months
- Multiple theatres with varying characteristics
- Audience influenced by holidays, weekends, theatre type, and booking trends
- Some theatres closed on certain days 
- Anonymized data with approximate geographical coordinates

## Approaches

### Version 1: Machine Learning Approach

**Strategy**: Supervised learning with extensive feature engineering

**Key Components:**
- **Feature Engineering**: 
  - Calendar features (day of week, month, quarter, holidays)
  - Lag features (7, 14, 21, 28 days)
  - Rolling statistics (7, 14, 28-day windows)
  - Same-day-of-week historical patterns
  - Theatre-level aggregations
  
- **Model Selection**:
  - Tested multiple algorithms: Linear Regression, Ridge, Lasso, Random Forest, Gradient Boosting, XGBoost, LightGBM
  - XGBoost selected as best performer
  - Time-series cross-validation for robust evaluation
  
- **Hybrid Forecasting**:
  - Combined ML predictions with statistical baseline
  - Optimized weighted ensemble (model + baseline)
  - Multi-step ahead forecasting with iterative prediction

### Version 2: Statistical Approach 

**Strategy**: Pure statistical methods without model training

**Key Components:**
- **Feature Engineering**:
  - Outlier capping (1st-99th percentile)
  - Calendar features
  - Temporal patterns
  
- **Statistical Features**:
  - Theatre-level: mean, median by various dimensions
  - Same-day-of-week lags (7, 14, 21, 28 days)
  - Weighted 4-week historical patterns
  - Recent trend indicators (7, 14, 21, 28-day rolling means)
  - Global fallbacks for unseen patterns
  
- **Three-Strategy Ensemble**:
  - Conservative: Emphasizes theatre-specific historical mean
  - Balanced: Equal weight to multiple signals
  - Aggressive: Prioritizes recent trends
  - Adaptive weighting based on data availability
  
- **Final Prediction**:
  - Weighted ensemble based on theatre data count
  - 2-day rolling smoothing for stability
  - Non-negative predictions

## Insights

1. **Simpler is better**: Statistical methods outperformed complex ML models
2. **Temporal patterns matter**: Same-day-of-week patterns are strong predictors
3. **Recency vs. History**: Balance between recent trends and historical averages is crucial
4. **Data-adaptive**: Different theatres need different prediction strategies
5. **Smoothing helps**: Rolling averages reduce prediction volatility

## Repository Structure

Here is the folder structure

```
├── README.md
├── 23f1001691-notebook-t32025 - Version 1.ipynb   # ML solution 
├── 23f1001691-notebook-t32025 - Version 2.ipynb   # Statistical solution
├── LICENSE
```

## Results


| Approach | Score | Key Technique |
|----------|-------|---------------|
| Version 1 | 0.39455 | XGBoost + Hybrid Forecasting |
| Version 2 | 0.43325 | Statistical Ensemble |


**Winner**: Version 2 - Statistical Approach

## License

This project is part of an academic competition submission.

## Author

Academic ID: 23f1001691 - Deebhika Kumaran