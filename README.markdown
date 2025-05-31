# IEEE-CIS Fraud Detection Kaggle Challenge

## Overview
This repository contains the code and documentation for our submission to the [IEEE-CIS Fraud Detection Kaggle competition](https://www.kaggle.com/competitions/ieee-fraud-detection). The goal of the project was to develop a predictive model to identify fraudulent credit card transactions using a large dataset provided by IEEE-CIS. Our approach leverages advanced feature engineering, data preprocessing, and machine learning models to achieve robust fraud detection with a focus on balancing precision and recall.

## Project Objectives
- Build a model to classify transactions as fraudulent or legitimate.
- Minimize false positives (legitimate transactions flagged as fraud) and false negatives (fraudulent transactions missed).
- Achieve a high Area Under the ROC Curve (AUC) score to handle the highly imbalanced dataset (96.5% non-fraudulent, 3.5% fraudulent).

## Dataset
The dataset consists of millions of credit card transactions split into four files:
- **Transaction Data**: Includes features like `TransactionID`, `TransactionAmt`, `ProductCD`, `card1`-`card6`, `addr1`, `addr2`, `P_emaildomain`, `R_emaildomain`, and engineered features (`Vxxx`, `C1`-`C14`, `D1`-`D15`, `M1`-`M9`).
- **Identity Data**: Provides additional details like `DeviceType`, `DeviceInfo`, and `id_01`-`id_38` for a subset of transactions.
- The datasets are linked via `TransactionID`.

## Approach
### Data Preprocessing
- **Missing Values**: Handled by imputing numerical features with median/mean and categorical features with placeholders (e.g., -999 or "missing").
- **Outlier Removal**: Capped or removed outliers in features like `TransactionAmt`, `dist1`, and `dist2`.
- **Feature Engineering**:
  - Extracted temporal features (`day`, `hour`) from `TransactionDT`.
  - Created aggregated features like `TransactionAmt_to_mean_card1`.
  - Encoded categorical variables using label encoding.
  - Applied Principal Component Analysis (PCA) to reduce the dimensionality of `V` features, retaining 98.6% variance with 30 components.
- **Data Integration**: Merged transaction and identity datasets on `TransactionID`.

### Modeling
- **Algorithms**:
  - **LightGBM**: Primary model due to its efficiency and ability to handle large datasets and categorical features. Achieved an AUC of 0.946 (training) and 0.926 (test).
  - **CatBoost**: Evaluated for its performance with categorical NaNs, achieving a Kaggle private score of 0.892948 and public score of 0.927335.
  - Other models like Decision Trees, Random Forests, and Gradient Boosting were tested as baselines.
- **Hyperparameter Tuning**: Performed using `RandomizedSearchCV` with stratified k-fold cross-validation for LightGBM.
- **Evaluation Metrics**: Focused on ROC-AUC due to class imbalance, supplemented by accuracy, precision, recall, and F1-score.

### Key Findings
- Feature engineering (temporal and aggregated features) significantly improved model performance.
- LightGBM outperformed CatBoost in training speed, while CatBoost showed competitive Kaggle scores but lower precision.
- Transaction amounts and engineered features were the most influential predictors.

## Results
- **LightGBM**: AUC of 0.946 (training) and 0.926 (test).
- **CatBoost**: Kaggle private score of 0.892948, public score of 0.927335.
- The models effectively balanced precision and recall, minimizing false positives to avoid customer inconvenience and false negatives to reduce financial losses.

## Repository Structure
```
├── data/                    # Dataset files (not included due to size)
├── notebooks/               # Jupyter notebooks for data exploration and modeling
├── src/                     # Source code for preprocessing, feature engineering, and modeling
├── submission/              # Submission files (e.g., submission.csv)
├── README.md                # This file
└── report.pdf               # Detailed project report
```

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/ieee-fraud-detection.git
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Ensure you have the dataset from [Kaggle](https://www.kaggle.com/competitions/ieee-fraud-detection/data).

## Usage
1. Run data preprocessing and feature engineering scripts in `src/`.
2. Train models using the provided notebooks or scripts in `notebooks/` or `src/`.
3. Generate predictions and submission files using the trained models.

## Future Work
- Explore additional feature engineering techniques to capture more complex patterns.
- Incorporate real-time data for dynamic fraud detection.
- Investigate ensemble and hybrid models for improved performance.

## References
- IEEE-CIS. (2019). IEEE-CIS Fraud Detection. Kaggle. Retrieved February 20, 2025, from https://www.kaggle.com/competitions/ieee-fraud-detection

## Contributors
- Salam AL-KAISSI
- Mayra SUAREZ
- Le HA
- Junzhang ZHONG