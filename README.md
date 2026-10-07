# Diabetes Hospital Readmission Prediction
This project uses the Diabetes 130-US Hospitals dataset to predict hospital readmission. The goal is to compare differe
## Dataset
The dataset used in this project is the **Diabetes 130-US Hospitals for Years 1999–2008** dataset.
It contains **101,766 patient records** with information about hospital stays, diagnoses, medications, admission and dThe target variable is `readmitted`, which has three classes:
- `<30` — readmitted within 30 days
- `>30` — readmitted after 30 days
- `NO` — not readmitted
The target distribution is:
| Class | Count | Percentage |
|---|---:|---:|
| NO | 54,864 | 53.91% |
| >30 | 35,545 | 34.93% |
| <30 | 11,357 | 11.16% |
The dataset can be downloaded from Kaggle:
[Diabetes 130-US Hospitals Dataset] (https://www.kaggle.com/datasets/gigimolashkhia/diabetes-130-us-hospitals-for-years)
## Data Cleaning
The dataset was checked for missing values, categorical variables, constant columns, and possible identifier columns.
The following columns were removed:
- `encounter_id`
- `patient_nbr`
- `examide`
- `citoglipton`
- `weight`
- `max_glu_serum`
Values represented by `?` were converted to missing values.
For some categorical columns, missing values were replaced with `Not_Specified` or `Not_Measured`.
The final dataset was checked again for missing values and duplicate rows.
## Data Preprocessing
Several categorical variables were converted into numerical values.
Medication-related variables were encoded numerically, and `change` and `diabetesMed` were converted to binary values.
The three diagnosis columns (`diag_1`, `diag_2`, and `diag_3`) were grouped into broader disease categories based on thAfter encoding, the feature matrix contained **100 features**.
The data was split into training and test sets using an 80/20 split with stratification.
- Training set: 81,412 records
- Test set: 20,354 records
The target classes were label encoded and the features were standardized using `StandardScaler`.
## Models
Four main classification models were trained and compared:
- Logistic Regression
- Decision Tree
- Random Forest
- XGBoost
The models were evaluated using:
- Accuracy
- Macro F1
- Weighted F1
- Macro AUC
## Model Comparison
The initial results were:
| Model | Accuracy | Macro F1 | Weighted F1 | Macro AUC |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.575 | 0.359 | 0.506 | 0.644 |
| Decision Tree | 0.480 | 0.384 | 0.480 | 0.542 |
| Random Forest | 0.584 | 0.390 | 0.536 | 0.663 |
| XGBoost | 0.594 | 0.405 | 0.546 | 0.685 |
XGBoost had the highest overall scores among the four initial models.
## Handling Class Imbalance
The `<30` class was the smallest class in the dataset, so different approaches were tested to see how class weighting Balanced versions of Logistic Regression and Random Forest were tested, as well as a Random Forest model with custom cA weighted XGBoost model was also trained using balanced class weights.
The weighted XGBoost model achieved:
- Accuracy: **0.518**
- Macro F1: **0.455**
- Weighted F1: **0.527**
For the `<30` class, recall increased to **0.40**, compared with **0.03** in the initial XGBoost model. However, the ov## Hyperparameter Tuning
Hyperparameter tuning was performed for Random Forest using `GridSearchCV`.
The parameters included:
- `n_estimators`
- `max_depth`
- `min_samples_split`
The best Random Forest parameters were:
```text
n_estimators = 100
max_depth = None
min_samples_split = 2
```
XGBoost was also tuned using `GridSearchCV` with 3-fold cross-validation.
The parameters tested were:
- `n_estimators`: 100, 200
- `max_depth`: 3, 6
- `learning_rate`: 0.05, 0.1
The best XGBoost parameters were:
```text
n_estimators = 200
max_depth = 6
learning_rate = 0.1
```
The tuned XGBoost model achieved:
- Accuracy: **0.597**
- Macro F1: **0.417**
- Weighted F1: **0.554**
- Macro AUC: **0.687**
## Final Model Comparison
The final comparison was:
| Model | Accuracy | Macro F1 | Weighted F1 | Macro AUC |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.575 | 0.359 | 0.506 | 0.644 |
| Decision Tree | 0.480 | 0.384 | 0.480 | 0.542 |
| Random Forest | 0.584 | 0.390 | 0.536 | 0.663 |
| XGBoost | 0.594 | 0.405 | 0.546 | 0.685 |
| Tuned XGBoost | 0.597 | 0.417 | 0.554 | 0.687 |
The tuned XGBoost model gave the best results in the final comparison, although the improvement over the initial XGBoo## Feature Importance
Feature importance was examined for the tree-based models, including Random Forest and the tuned XGBoost model.
For XGBoost, some of the more important features included:
- `number_inpatient`
- `discharge_disposition_id`
- `diag_1_Pregnancy`
- `diabetesMed`
## Model Explainability with SHAP
SHAP was used to examine the predictions of the tuned XGBoost model.
A SHAP summary plot was used to look at the overall feature contributions. A dependence plot was also created for `numbThis helped provide a closer look at how the features were contributing to the model's predictions.
## Conclusion
The results show that predicting hospital readmission is difficult, especially for the `<30` class.
Among the initial models, XGBoost performed best. Tuning its hyperparameters improved the results slightly, with the fClass weighting improved the prediction of the minority `<30` class, but this came with a decrease in overall accuracyOverall, the project provided a comparison of several classification models and also included feature importance and SH## Technologies Used
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- SHAP
- Jupyter Notebook