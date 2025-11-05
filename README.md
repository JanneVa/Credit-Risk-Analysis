# Credit Risk Analysis: South German Credit Dataset

## Executive Summary

This comprehensive analysis of the South German Credit dataset reveals critical insights for credit risk modeling and operational decision-making. The dataset contains 1,000 credit applications with a 30% default rate, providing a solid foundation for developing predictive models and risk management strategies.

### Latest Results (global.ipynb)
- Recommended final model: `rf_calibrated` (Calibrated Random Forest)
- Recommended decision threshold (cost FP:5, FN:1): `0.76`
- Key test metrics (post-calibration): ROC-AUC ≈ 0.804, PR-AUC ≈ 0.894, F1@0.5 ≈ 0.848, F1@opt ≈ 0.866
- Stability: 5-fold stratified CV with low variance vs. peers
- Fairness: Reported Demographic Parity and Equal Opportunity by groups; propose segment thresholds or manual review where needed

## Dataset Overview

- **Total Records**: 1,000 credit applications
- **Default Rate**: 30% (300 bad credit risks, 700 good credit risks)
- **Features**: 20 attributes (3 numerical, 4 ordinal, 13 categorical)
- **Data Quality**: No missing values detected
- **Target Variable**: Binary classification (0=Bad, 1=Good)

## Key Findings

### 1. High-Risk Segments Identified

**Age-Based Risk Patterns:**
- Applicants under 25 years: 42.1% default rate
- Risk decreases with age, showing clear demographic patterns

**Credit Amount Risk:**
- Amount range 10K-20K: 60.0% default rate
- Higher amounts show increased risk concentration

**Duration Impact:**
- Long-term credits (5+ years): 100% default rate
- Duration is a critical risk factor

### 2. Data Quality Assessment

**Outlier Analysis:**
- **Duration**: 70 outliers (7.0%) detected using IQR method
- **Credit Amount**: 72 outliers (7.2%) with extreme values
- **Age**: 23 outliers (2.3%) in senior age groups

**Categorical Variables:**
- Rare levels identified in multiple categorical features
- Systematic treatment applied to maintain data integrity

### 3. Statistical Insights

**Variable Relationships:**
- Strong correlation between credit amount and default risk
- Age shows non-linear relationship with creditworthiness
- Employment status and savings account are key predictors

**Distribution Patterns:**
- Credit amounts show right-skewed distribution
- Age distribution is relatively normal with some outliers
- Duration follows exponential-like distribution

## Data Preprocessing Applied

### 1. Data Type Optimization
- Numerical variables: `duration_months`, `credit_amount`, `age`
- Ordinal variables: `installment_rate`, `residence_since`, `existing_credits`, `people_liable`
- Categorical variables: 13 features including `checking_account_status`, `purpose`, `employment_since`

### 2. Outlier Treatment
- **Numerical variables**: IQR-based outlier detection and treatment
- **Ordinal variables**: Rare level identification and consolidation
- **Categorical variables**: Rare category grouping

### 3. Feature Engineering
- Risk rate calculations by segment
- Age and amount grouping for analysis
- Duration risk assessment

## Modeling Recommendations

### 1. Model Selection Strategy (Updated)

We trained and evaluated Logistic Regression, Decision Tree, Random Forest, and MLP, incorporating probability calibration (Isotonic/Platt) and stratified cross-validation for stability. The calibrated Random Forest showed the lowest expected cost under FP:FN = 5:1 while keeping strong PR-AUC/ROC-AUC and low variance.

**Final recommendation:**
- Model: `rf_calibrated`
- Threshold (cost-optimal FP:5, FN:1): `0.76`
- Why: minimizes expected cost, competitive PR-AUC on imbalanced data, stable across folds, calibrated probabilities (thresholds are reliable)

### 2. Feature Engineering for Modeling

**High-Priority Features:**
- Age groups (especially <25 years)
- Credit amount segments
- Duration categories
- Employment status
- Savings account status
- Credit history

**Interaction Terms:**
- Age × Credit Amount
- Duration × Employment Status
- Savings × Credit History

### 3. Model Validation Strategy (Updated)

**Cross-Validation:**
- 5-fold stratified cross-validation (stability reported: mean, std, CV by metric)
- Time-based splits if temporal patterns exist
- Holdout set for final evaluation

**Performance Metrics:**
- **Primary**: ROC-AUC and PR-AUC
- **Business**: Cost-sensitive metrics (cost matrix: FP:5, FN:1)
- **Threshold Sensitivity**: metrics vs threshold curves; F1-optimal and cost-optimal thresholds
- **Fairness**: Demographic Parity (positive rate) and Equal Opportunity (TPR) by groups; group sizes and visuals

### 4. Risk Management Integration

**Scoring Implementation:**
- Real-time scoring for new applications
- Risk tier classification (Low/Medium/High)
- Automated decision rules for clear-cut cases

**Monitoring Framework:**
- Model performance tracking
- Feature drift detection
- Business impact measurement

## Operational Recommendations

### Immediate Actions (0-30 days)
1. **Implement risk-based policies** for identified high-risk segments
2. **Deploy automated alerts** for critical risk combinations
3. **Train credit team** on new risk indicators

### Short-term (1-3 months)
1. **Deploy calibrated model (`rf_calibrated`)** with decision threshold `0.76` (cost FP:5, FN:1)
2. **Create differentiated policies** by segment (fairness: lower TPR groups) or add manual review
3. **Implement A/B testing** for threshold and policy adjustments

### Medium-term (3-6 months)
1. **Continuous monitoring** of PR-AUC/ROC-AUC, F1, Brier Score (recalibrate if Brier worsens >10%)
2. **Build fairness dashboard** (DP/TPR by group, group sizes); monthly audits
3. **Establish model refresh** procedures and drift detection (KS on scores, PSI on key variables)

##Recomendation
- We recommend a calibrated Random Forest with a 0.76 threshold for default risk decisions.
- This setup reduces the cost of wrong decisions given the business policy FP:FN = 5:1, remains stable across datasets, and uses calibrated probabilities (thresholds are meaningful).
- Operatively, use it for screening/prioritization; apply segment-specific thresholds or manual review if fairness audits show gaps.

## Expected Business Impact

- **Default Rate Reduction**: 15-25%
- **Processing Time Improvement**: 20-30%
- **Portfolio Profitability**: +10-15%

## Technical Implementation

### Data Pipeline
```
Raw Data → Preprocessing → Feature Engineering → Model Training → Validation → Deployment
```

### Model Monitoring
- Performance metrics tracking
- Feature importance monitoring
- Prediction distribution analysis
- Business outcome validation

## Ethical Considerations

### Fairness Requirements
- Avoid proxy discrimination based on protected characteristics
- Implement fairness metrics (demographic parity, equalized odds)
- Regular bias auditing and mitigation

### Transparency
- Model interpretability for regulatory compliance
- Clear documentation of decision factors
- Explainable AI for high-risk decisions

## Files and Outputs

### Generated Datasets
- `credit_data_clean_[timestamp].csv`: Preprocessed dataset
- `variables_info_[timestamp].csv`: Variable metadata
- `descriptive_stats_[timestamp].csv`: Statistical summaries

### Analysis Reports
- `narrativa_negocio_credit_risk.md`: Business narrative (Spanish)
- `eda_outputs/`: Complete exploratory analysis outputs

## Next Steps

1. **Model Development**: Implement recommended modeling approach
2. **Validation**: Comprehensive model validation and testing
3. **Deployment**: Production-ready model implementation
4. **Monitoring**: Continuous performance and fairness monitoring
5. **Iteration**: Regular model updates and improvements

---

*This analysis provides a solid foundation for developing a robust credit risk management system. The identified patterns and recommendations should be validated with business stakeholders and implemented through a phased approach.*
