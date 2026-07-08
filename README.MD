# MIS-690 Capstone — Guest Segmentation at Target Corporation
 
A data-driven guest segmentation framework built for Target Corporation, using RFM feature engineering, K-Means clustering, and Random Forest regression to identify and prioritize high-value guests. Completed as the MIS-690 Capstone Project for the M.S. in Business Analytics program at Grand Canyon University.
 
**Authors:** Hunter Sarkis and Illen Ortet

**Program:** M.S. Business Analytics, Colangelo College of Business, Grand Canyon University
 
## Business Problem
 
Target serves millions of guests across in-store and digital channels but lacks a reliable, data-driven framework for segmenting its guest base by purchasing behavior. Marketing campaigns are designed to appeal broadly rather than addressing the distinct needs of different guest groups, leading to inefficient budget allocation, lower campaign conversion, and missed retention opportunities.
 
## Dataset
 
Because Target's proprietary guest data isn't publicly available, this project uses the **Olist Brazilian E-Commerce Public Dataset** (Kaggle, Creative Commons license) as a behavioral proxy. The dataset contains approximately 100,000 orders placed between October 2016 and August 2018. After filtering to delivered orders only, the analytical dataset contains **93,357 unique guests**.
 
## Methodology
 
The project follows the CRISP-DM framework across two analytical phases:
 
1. **Segmentation** — Seven behavioral features (Recency, Frequency, Monetary value, Average Review Score, Average Freight Value, Payment Type, Customer Tenure) were engineered from raw order-level data and standardized. K-Means clustering (validated via the elbow method and silhouette scoring) identified an optimal **K=2** solution:
   - **Single-Purchase Guests** — 90,551 guests (97%)
   - **Loyal Guests** — 2,806 guests (3%)
 
2. **Prediction** — Within each segment, Decision Tree, Random Forest, and XGBoost regressors were tuned via GridSearchCV to predict guest Monetary value. **Random Forest** was selected as the final model for both segments based on stability across partitions and resistance to overfitting.
 
## Key Findings
 
- A Welch's t-test confirmed loyal guests spend **nearly 2x more** on average than single-purchase guests, t(2885.16) = 24.50, p < .001.
- Tuned Random Forest achieved holdout R² of **0.601** (Cluster 0) and **0.313** (Cluster 1), with 5-fold cross-validation confirming stability (mean R² of 0.604 and 0.443, respectively).
- **Average Freight Value** is the dominant predictor of spend in both segments, but may partly reflect a mechanical relationship with order size rather than pure behavioral signal — flagged directly as a risk in the deployment monitoring plan.
- External validation against the UCI Online Retail dataset (a structurally different, UK-based retail dataset) showed a sharp performance drop, confirming the model requires retraining on Target's actual U.S. guest data before production use.
 
## Repository Structure
 
```
├── notebooks/
│   ├── topic2_data_understanding.ipynb              # Data collection, RFM engineering, EDA, hypothesis testing
│   ├── topic3_data_preparation.ipynb                 # RFM-Plus feature set (7 features), Simpson's Rule check, scaling
│   ├── topic4_model_building.ipynb                   # K-Means clustering (K=2), correlation analysis, DT/RF/XGBoost training
│   └── topics5-8_model_evaluation_and_deployment.ipynb  # Hyperparameter tuning, sensitivity simulation, bootstrap
│                                                        resampling, holdout validation, synthetic stress testing,
│                                                        5-fold cross-validation, and UCI external validation —
│                                                        supporting the Model Evaluation, External Verification,
│                                                        and Deployment sections of the thesis
├── outputs/
│   ├── rfm_table.csv, rfm_plus_table.csv, rfm_clustered_table.csv   # Feature tables at each pipeline stage
│   ├── model_results_cluster0.csv, model_results_cluster1.csv       # Model comparison results by segment
│   ├── holdout_results.csv, bootstrap_cluster1_results.csv          # Validation results
│   ├── sensitivity_cluster0.csv, sensitivity_cluster1.csv           # Freight sensitivity simulation
│   ├── stress_test_results.csv                                      # Synthetic stress test results
│   ├── final_model_recommendation.csv                                # Final model selection by segment
│   ├── fig1–fig13 (.png)                                             # All report figures
│   └── topic2_raw_data.xlsx, topic3_dashboard.pbix                   # Raw RFM export and Power BI dashboard
├── .gitignore
└── README.md
```
 
> **Note:** Raw Olist source data is excluded from this repository due to file size. Download it directly from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) and place it in a local `data/` folder to reproduce the notebooks from scratch.
 
## Tech Stack
 
Python (pandas, scikit-learn, XGBoost, matplotlib) · Jupyter Notebook · Microsoft Power BI · Microsoft Excel
 
## Deployment Recommendation
 
A phased, batch-scoring deployment is recommended over real-time prediction, with quarterly model refreshes integrated into existing marketing and reporting systems. Deployment governance, retraining triggers, and rollback rules are defined in the full thesis. Based on a conservative 2% churn-reduction assumption, the model projects a positive first-year return on investment under a lightweight deployment approach.
 
## Full Report
 
The complete capstone thesis, including literature review, full validation methodology, deployment governance framework, and cost-benefit analysis, is available as a companion Word document (not included in this repository).