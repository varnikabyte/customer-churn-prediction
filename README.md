# Customer Churn Prediction

Predicts whether a telecom customer will churn, using the Telco Customer Churn dataset (7,043 customers, 21 features).

## Project Workflow

1. *Data Cleaning*
   - Dropped customerID (not predictive)
   - Converted TotalCharges from object to float; 11 blank values (customers with 0 tenure) filled with 0

2. *EDA*
   - Numerical features: histograms and box plots (tenure, MonthlyCharges, TotalCharges)
   - Categorical features: count plots for all categorical columns
   - Bivariate analysis: each feature vs Churn, plus a correlation heatmap for numerical features
   - Found class imbalance in the target (~73% No, ~27% Yes)

3. *Preprocessing*
   - Train-test split (80/20)
   - Label encoding for binary columns (gender, Partner, Dependents, PhoneService, PaperlessBilling)
   - One-hot encoding for multi-category columns (MultipleLines, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies, Contract, PaymentMethod)
   - SMOTE applied to the training set only, to balance the churn classes
   - Feature scaling (StandardScaler) on tenure, MonthlyCharges, TotalCharges, fit on train only

4. *Model Training*
   Compared 5 models with 5-fold cross-validation:

   | Model | CV Accuracy |
   |---|---|
   | Logistic Regression (L2) | 0.79 |
   | Logistic Regression (L1) | 0.78 |
   | Decision Tree | 0.80 |
   | Random Forest | 0.84 |
   | XGBoost | 0.83 |

5. *Model Selection*
   Random Forest had higher cross-validation accuracy, but both Random Forest and XGBoost scored the same on the test set (0.79 accuracy). Since recall on the churn class matters more for this problem, XGBoost was chosen for its better recall (0.55 vs 0.53) and F1-score (0.58 vs 0.57).

   *Final test performance (XGBoost):*
   | Class | Precision | Recall | F1-score |
   |---|---|---|---|
   | No Churn (0) | 0.85 | 0.88 | 0.86 |
   | Churn (1) | 0.62 | 0.55 | 0.58 |

6. *Predictive System*
   The trained model, encoders, and scaler are saved with pickle. A predict_churn() function takes a new customer's raw details and returns a churn prediction with probability.

## Files
- customer_churn_model.pkl — trained XGBoost model + expected feature names
- encoders.pkl — label encoders for binary columns
- onehot_encoder.pkl — one-hot encoder for multi-category columns
- scaler.pkl — fitted StandardScaler for numerical columns

## Tech Stack
Python, pandas, NumPy, scikit-learn, XGBoost, imbalanced-learn (SMOTE), matplotlib, seaborn
