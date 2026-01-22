# Customer Churn Prediction

A machine learning project that predicts telecom customer churn and identifies key drivers of customer attrition, delivering $2.8M in annual business value through proactive retention.

## Table of Contents
- [Project Overview](#project-overview)
- [Business Impact](#business-impact)
- [Dataset](#dataset)
- [Key Findings](#key-findings)
- [Technical Approach](#technical-approach)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Results](#results)
- [Future Improvements](#future-improvements)

## Project Overview

This project analyzes customer churn patterns in a telecommunications company and builds a predictive model to identify at-risk customers before they leave. The analysis combines exploratory data analysis, feature engineering, and machine learning to deliver actionable business insights.

**Problem:** 26.5% annual churn rate costing $3.7M in lost revenue

**Solution:** Predictive model that catches 78.9% of churners, enabling proactive retention

**Result:** $2.8M net annual value (23.6x ROI)

## Business Impact

### Quantified Results
- **Revenue Protected:** $2.95M annually
- **Retention Cost:** $120K annually
- **Net Benefit:** $2.83M annually
- **ROI:** 23.6 to 1

### Key Insights Discovered
1. Month-to-month customers churn at **15x higher rate** than two-year contract customers
2. First 24 months is critical risk period
3. Rapid service adoption (3+ services in 6 months) signals **54.8% churn risk**, not engagement
4. Premium pricing acceptable only with proportional service value

## Dataset

**Source:** Kaggle - Telco Customer Churn Dataset

**Size:** 7,043 customer records

**Features:** 21 original features including:
- Customer demographics (gender, age, dependents)
- Service subscriptions (internet, phone, streaming, security)
- Account information (tenure, contract type, payment method)
- Billing data (monthly charges, total charges)
- Target variable: Churn (Yes/No)

**Class Distribution:** 73.5% retained, 26.5% churned

## Key Findings

### Churn Drivers (Ranked by Importance)

1. **Contract Type** - Strongest predictor
   - Month-to-month: 42.7% churn
   - One year: 11.3% churn
   - Two year: 2.8% churn

2. **Customer Tenure** - Second strongest
   - Average tenure of churners: 18 months
   - Average tenure of retained: 38 months
   - 52% difference

3. **Pricing Perception**
   - Churners pay 21.5% more monthly ($74 vs $61)
   - High price without service value drives churn

4. **Service Adoption Rate** - Counter-intuitive finding
   - Fast adopters (3+ services in 6 months): 54.8% churn
   - Slow adopters: 4.8% churn
   - Indicates bill shock, not engagement

### High-Risk Segments

**Segment 1:** New customers (0-24 months) + Month-to-month + Premium pricing
- Size: 1,251 customers (18% of base)
- Churn Rate: 62.7%
- Revenue at Risk: $1.6M

**Segment 2:** Fast service adopters
- Size: 1,342 customers (19% of base)
- Churn Rate: 54.8%
- Revenue at Risk: $1.5M

## Technical Approach

### 1. Exploratory Data Analysis
- Univariate and bivariate analysis
- Statistical hypothesis testing (t-tests)
- Correlation analysis
- Customer segmentation

**Key Tools:** pandas, numpy, matplotlib, seaborn, scipy

### 2. Feature Engineering

Created 7 new features based on domain insights:

| Feature | Description | Correlation with Churn |
|---------|-------------|----------------------|
| ContractRiskScore | Composite 0-8 score combining contract, tenure, price | +0.468 (strongest) |
| ServiceAdoptionRate | Services added per month of tenure | +0.356 |
| TenureBucket | Categorical risk segments (New, Early, Established, Loyal) | N/A (categorical) |
| ChargeCategory | Price tier (Budget, Standard, Premium) | N/A (categorical) |
| HasFiberOptic | Binary flag for premium internet | +0.307 |
| AvgRevenuePerService | Monthly charge / number of services | +0.313 |
| ServiceCount | Total services subscribed | -0.067 (weak) |

### 3. Model Development

**Models Tested:**
- Logistic Regression (selected)
- Random Forest
- XGBoost

**Evaluation Metrics:**
- Recall (primary): 78.9%
- ROC AUC: 0.833
- Precision: 49.2%
- F1 Score: 0.606

**Model Selection Rationale:**
Logistic Regression selected over tree-based models because:
- Best business value ($559K vs $548K for Random Forest)
- Highest recall (priority metric for this use case)
- Most interpretable for stakeholders
- Engineered features already captured non-linear patterns

### 4. Model Interpretation

**Top 5 Most Important Features:**
1. Contract type (Two year) - Coefficient: -1.48
2. Charge category (Premium) - Coefficient: -0.84
3. Charge category (Standard) - Coefficient: -0.79
4. Contract type (One year) - Coefficient: -0.73
5. Tenure bucket (Loyal) - Coefficient: +0.52

## Installation

### Prerequisites
```bash
Python 3.8+
Jupyter Notebook
```

### Setup
```bash
# Clone repository
git clone https://github.com/yourusername/customer-churn-prediction.git
cd customer-churn-prediction

# Install dependencies
pip install -r requirements.txt
```

### Dependencies
```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=1.0.0
xgboost>=1.5.0
scipy>=1.7.0
```

## Usage

### Running the Analysis

**Step 1: Data Exploration**
```bash
jupyter notebook notebooks/01_exploration.ipynb
```

**Step 2: Feature Engineering**
```bash
jupyter notebook notebooks/02_feature_engineering.ipynb
```

**Step 3: Model Building**
```bash
jupyter notebook notebooks/03_modeling.ipynb
```

### Using the Trained Model
```python
import pandas as pd
import pickle

# Load model
with open('models/logistic_regression_model.pkl', 'rb') as f:
    model = pickle.load(f)

# Load new customer data
new_customers = pd.read_csv('new_customer_data.csv')

# Predict churn probability
churn_probabilities = model.predict_proba(new_customers)[:, 1]

# Flag high-risk customers
high_risk = new_customers[churn_probabilities > 0.5]
```

## Project Structure
```
customer-churn-prediction/
│
├── data/
│   ├── telco_churn.csv                 # Original dataset
│   ├── telco_churn_cleaned.csv         # Cleaned data
│   └── telco_churn_engineered.csv      # Final feature set
│
├── notebooks/
│   ├── 01_exploration.ipynb            # EDA and insights
│   ├── 02_feature_engineering.ipynb    # Feature creation
│   └── 03_modeling.ipynb               # Model training and evaluation
│
├── models/
│   └── (trained models will be saved here)
│
├── visualizations/
│   └── (key charts and figures)
│
├── EXECUTIVE_SUMMARY.md                # Business summary
├── README.md                           # This file
└── requirements.txt                    # Python dependencies
```

## Results

### Model Performance

| Metric | Score | Business Interpretation |
|--------|-------|------------------------|
| Recall | 78.9% | Catches 295 of 374 churners in test set |
| Precision | 49.2% | 49% of churn predictions are correct |
| ROC AUC | 0.833 | Strong discrimination ability |
| Accuracy | 72.8% | Overall prediction accuracy |

### Confusion Matrix (Test Set)

|  | Predicted Stay | Predicted Churn |
|---|----------------|-----------------|
| **Actual Stay** | 729 | 304 |
| **Actual Churn** | 79 | 295 |

### Business Impact

**On test set of 1,407 customers:**
- Correctly identified churners: 295 (save $590,000)
- Missed churners: 79 (lose $158,000)
- False alarms: 304 (waste $30,400)
- **Net value: $559,600**

**Scaled to full customer base:**
- Annual net benefit: $2,830,000
- ROI: 23.6 to 1

## Future Improvements

### Model Enhancements
1. **Ensemble stacking** - Combine predictions from multiple models
2. **Hyperparameter tuning** - Grid search for optimal parameters
3. **Time-based features** - Incorporate seasonal patterns
4. **Customer lifetime value prediction** - Prioritize high-value customers

### Data Enhancements
1. **Interaction data** - Call center contacts, complaints, service issues
2. **Usage patterns** - Data consumption, call minutes, streaming hours
3. **Competitive intelligence** - Local market competition, pricing
4. **Customer feedback** - Satisfaction scores, NPS

### Deployment
1. **Real-time scoring API** - REST endpoint for instant predictions
2. **Automated retraining pipeline** - Monthly model updates
3. **A/B testing framework** - Test retention strategies
4. **Monitoring dashboard** - Track model performance and business metrics

## Contributing

Contributions are welcome. Please:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a pull request

## License

This project is licensed under the MIT License.

## Contact

**Author:** Nikhil Thamma

**Email:** nthamma1@babson.edu

**LinkedIn:** https://www.linkedin.com/in/nikhilthamma

**Portfolio:** https://nikhilthamma.replit.app/

## Acknowledgments

- Dataset: IBM Sample Data Sets via Kaggle
- Inspiration: Real-world business analytics applications
- Tools: scikit-learn, XGBoost, pandas ecosystem

---

**Last Updated:** January 2026
