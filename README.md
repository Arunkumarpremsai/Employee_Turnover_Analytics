#  Employee Turnover Analytics Pipeline

This repository contains my end-to-end data science project focused on predicting employee attrition and discovering the underlying factors behind workforce turnover. Here is a breakdown of what I built and executed in this project.

---

## What I Did in This Project

### 1. Data Auditing & Quality Verification
* **Ingested Corporate Records:** I processed a dataset containing 14,999 individual employee records across 10 behavioral and evaluation features.
* **Quality Inspections:** I conducted strict automated data quality checks which verified that the dataset contained zero missing values.
* **Imbalance Assessment:** I analyzed the baseline distribution of the target variable (`left`), identifying a significant class imbalance where 23.8% of the workforce had left the company.

### 2. Exploratory Data Analysis (EDA) & Insights
* **Statistical Visualizations:** I constructed distribution profiles for critical employee dimensions, including satisfaction levels, last evaluation scores, and average monthly hours.
* **Correlation Mapping:** I generated a comprehensive numerical correlation heatmap to capture direct relationships between features and attrition.
* **Discovered Attrition Triggers:** I mapped workload metrics against turnover using an engineered bar chart. 
  * *Insight:* I discovered a stark non-linear correlation: employees assigned to only 2 projects or 5+ projects exhibited massive attrition rates, revealing that a workload of 3–4 projects serves as the optimal threshold for employee retention.

### 3. Unsupervised Talent Segmentation (K-Means Clustering)
* **Cohort Isolation:** I filtered the dataset to isolate only the 3,571 employees who chose to leave the organization[cite: 1].
* **K-Means Clustering Pipeline:** I implemented a K-Means Clustering model ($k=3$) using `satisfaction_level` and `last_evaluation` to identify distinct retention risk profiles.
* **Segment Profiling:** I extracted the exact centroids and sizes of these groups:
  * **Cluster 0 (1,650 employees):** Exhibited low-to-moderate satisfaction (~41.0%) alongside mid-tier evaluations (~51.7%).
  * **Cluster 1 (977 employees):** Represented satisfied high-performers leaving the firm despite elite evaluations (~91.2%) and high satisfaction (~80.9%).
  * **Cluster 2 (944 employees):** Revealed severely burned-out talent, showing rock-bottom satisfaction (~11.1%) despite holding stellar evaluation scores (~86.9%).

### 4. Data Preprocessing & Leakage Prevention
* **Categorical Transformation:** I transformed multi-class textual fields (`sales` department and `salary` categories) into a clean, model-ready format using dummy variable encoding via `get_dummies`.
* **Stratified Validation Inception:** I set up a stratified 80:20 train-test split, separating 11,999 samples for training and locking away 3,000 completely pure samples for testing.
* **Synthetic Minority Over-sampling (SMOTE):** To eliminate major classification bias caused by the class imbalance, I implemented SMOTE. I carefully applied it **only to the training data**, balancing both classes to exactly 9,142 instances each while protecting the test data from any data leakage.

### 5. Model Architecture & Cross-Validation
* **Classifier Benchmarking:** I built and trained three unique classification models to predict attrition risk:
  1. **Logistic Regression** (optimized with balanced class weights).
  2. **Random Forest Classifier** (configured with 100 estimators and balanced bootstrapping).
  3. **Gradient Boosting Classifier** (tuned with 200 estimators, 0.1 learning rate, and a max depth of 4).
* **Rigorous Cross-Validation:** Instead of relying on a single test score, I set up a strict 5-Fold Stratified Cross-Validation framework to measure model reliability.
* **Evaluation Framework:** I designed the pipeline to calculate out-of-fold performance, tracking Confusion Matrices, Classification Reports, F1-Scores, and ROC-AUC metrics to evaluate the exact predictive power of each algorithm.

---
