# Regression, Regularisation & Sparse Feature Selection

A hands-on practice project on core supervised-learning techniques with **scikit-learn**: KNN regression, regularised linear models (Lasso / Ridge / ElasticNet), and L1-penalised logistic regression for feature selection on high-dimensional gene-expression data.

## What's covered

| # | Topic | Dataset | Key idea |
|---|-------|---------|----------|
| 1 | **KNN regression** | California Housing (20,640 × 8) | Effect of `k`, distance weighting and distance metric |
| 2 | **Regularised linear regression** | UCI Student Performance (395 × 32) | OLS vs L1 vs L2 vs ElasticNet, one-hot encoding |
| 3 | **Sparse logistic regression** | Gene expression (172 × ~21,000) | L1 for feature selection when p ≫ n, handling missing data |

👉 Notebook: [`notebooks/regression_regularisation_feature_selection.ipynb`](notebooks/regression_regularisation_feature_selection.ipynb)

## Results at a glance

**KNN regression (R², test set)**

| Setting | R² |
|---|---|
| k = 3 | 0.644 |
| k = 30 | 0.671 |
| k = 300 | 0.603 |
| k = 30, distance-weighted, euclidean | 0.677 |
| k = 30, distance-weighted, cosine | 0.670 |
| k = 30, distance-weighted, **manhattan** | **0.713** |

**Linear models on student grades (R², test set)**

| Model | R² | Non-zero coefs |
|---|---|---|
| Linear Regression | 0.733 | 58 / 58 |
| **Lasso (L1)** | **0.738** | **2 / 58** |
| Ridge (L2) | 0.734 | 58 / 58 |
| ElasticNet | 0.698 | 3 / 58 |

**L1 logistic regression on gene expression (accuracy, test set)**

| Missing-data strategy | C | Accuracy | Genes selected |
|---|---|---|---|
| Drop rows | 0.05 | 0.872 | 3 |
| Drop rows | 10 | 0.949 | 236 |
| Drop columns | 0.05 | 0.930 | 10 |
| **Drop columns** | **10** | **0.977** | 185 |

## Key learnings

- **Scale before distance-based models.** KNN is meaningless on unscaled features.
- **Fit preprocessing on the training set only** to avoid data leakage, and align one-hot columns between the train and test splits.
- **The bias–variance trade-off shows up clearly in KNN:** small `k` overfits and very large `k` underfits.
- **Lasso does feature selection for free.** It matched OLS while using only 2 of 58 features.
- **L1 regularisation strength (`C`) controls sparsity**, which lets you trade interpretability (a handful of genes) against accuracy.
- **When p ≫ n, keep the samples and drop features with missing values.** Each sample is worth far more than any single feature.

## Tech stack

Python · NumPy · pandas · scikit-learn · Jupyter

## Running it

```bash
pip install -r requirements.txt
jupyter notebook notebooks/regression_regularisation_feature_selection.ipynb
```

See [`data/README.md`](data/README.md) for dataset sources. The California Housing data is downloaded automatically by scikit-learn.
