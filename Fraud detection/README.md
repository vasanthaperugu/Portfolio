
# Fraud Detection Model Evaluation

Model evaluation for imbalanced classification in fraud detection, achieving 53.6% cost reduction through threshold optimization.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3.0-orange.svg)
![Cost Reduction](https://img.shields.io/badge/Cost%20Reduction-53.6%25-success.svg)

## Overview

Built machine learning models for fraud detection with severe class imbalance (98% legitimate, 2% fraud). Demonstrated why accuracy fails and optimized thresholds based on business costs ($5,000 per missed fraud vs $50 per false alarm).

**Result: $133,400 savings (53.6% cost reduction)**

## Dataset

Synthetic fraud detection dataset: 10,000 bank transactions, 2.4% fraud rate, 20 features. Generates automatically via scikit-learn.

## Model Architecture

Gradient Boosting with 100 estimators and max depth 5. Used stratified cross-validation and cost-based threshold optimization (0.07 vs default 0.50).

## Results

| Model | Recall | Precision | Cost |
|-------|--------|-----------|------|
| Gradient Boosting (optimized) | 54.2% | 48.1% | **$106,600** |
| Gradient Boosting (default) | 39.6% | 73.1% | $145,350 |
| Random Forest | 20.8% | 100% | $120,000+ |
| Dummy Classifier | 0% | - | $240,000 (baseline) |

Optimal Threshold: **0.07**

## Key Findings

- Dummy classifier achieves 97.60% accuracy while catching 0% fraud - accuracy is meaningless for imbalanced data
- Threshold optimization from 0.50 to 0.07 saves $38,750 by exploiting 100:1 cost ratio
- Gradient Boosting outperforms Random Forest despite lower AUC (0.867 vs 0.878) - recall matters more

## How to Run

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
jupyter notebook fraud_detection_evaluation.ipynb
```

Dataset generates synthetically - no downloads needed.

## Technologies

Python, scikit-learn, NumPy, Pandas, Matplotlib, Seaborn

---

**Note:** Techniques apply to any imbalanced classification problem (medical diagnosis, anomaly detection, churn prediction).
