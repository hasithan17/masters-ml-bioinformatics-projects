# **Predicting Parkinson’s Disease from Voice Biomarkers Using Machine Learning**  
**Author:** Hasitha Nannapaneni  

---

## **Overview**
This project applies **machine learning** to acoustic voice biomarkers to distinguish Parkinson’s disease (PD) patients from healthy controls. Using the **UCI Parkinson’s Voice Dataset**, the workflow integrates:

- Exploratory data analysis  
- Feature scaling and imputation  
- Logistic regression with L2 regularization  
- Stratified cross‑validation  
- PCA for dimensionality reduction  
- Linear regression for feature interdependence analysis  

The goal is to evaluate whether interpretable ML models can reliably detect PD from non‑invasive voice recordings.

---

## **Dataset**
- Source: **UCI Machine Learning Repository**  
- URL: `https://archive.ics.uci.edu/ml/datasets/parkinsons` [(archive.ics.uci.edu in Bing)](https://www.bing.com/search?q="https%3A%2F%2Farchive.ics.uci.edu%2Fml%2Fdatasets%2Fparkinsons")  
- Samples: **195 sustained phonation recordings**  
- Subjects: **31 individuals**  
- PD samples: **147**  
- Healthy controls: **48**  
- Features: **22 acoustic biomarkers**, including:  
  - jitter variants  
  - shimmer variants  
  - harmonic‑to‑noise ratio (HNR)  
  - fundamental frequency (Fo)  
  - nonlinear dynamical measures  

### **Target**
- `status`  
  - **1 = Parkinson’s disease**  
  - **0 = healthy control**

---

## **Preprocessing**
To prepare the dataset for modeling:

- **Median imputation** for missing values  
- **StandardScaler** for normalization  
- **Stratified train/test split** to preserve class balance  
- **ColumnTransformer** pipeline for clean preprocessing  

These steps address skewed distributions, wide dynamic ranges, and class imbalance.

---

## **Exploratory Data Analysis**
Key findings from EDA:

- Strong skew in jitter and shimmer distributions  
- High collinearity among acoustic features  
- Correlation heatmap shows jitter–shimmer–HNR clusters  
- PCA reveals that **PC1 + PC2 explain 70.6% of variance**  
- PCA scatter plot shows partial separation between PD and controls  

These patterns align with known dysphonia characteristics in PD.

---

## **Modeling Approach**

### **1. Logistic Regression (Primary Classifier)**
- L2 regularization  
- Hyperparameter tuning via **GridSearchCV**  
- Cross‑validation using **StratifiedKFold (5 folds)**  
- Evaluation metrics:  
  - Accuracy  
  - Precision  
  - Recall  
  - F1‑score  
  - ROC‑AUC  
  - Balanced accuracy  
  - Confusion matrix  

### **2. PCA + Logistic Regression**
- PCA (n_components = 2 and 8)  
- PCA‑based classifier achieved **ROC‑AUC ≈ 0.902**, nearly identical to full‑feature model  

### **3. Linear Regression (Exploratory)**
- Modeled shimmer as a function of jitter + HNR  
- Achieved **R² = 0.995**, **RMSE = 0.001**  
- Demonstrates strong interdependence among acoustic biomarkers  

---

## **Results**

### **Classification Performance**
- **Mean CV ROC‑AUC:** 0.900  
- **Best CV ROC‑AUC:** 0.905  
- **Test Accuracy:** 0.847  
- **Precision:** 0.907  
- **Recall:** 0.886  
- **F1‑score:** 0.897  
- **Test ROC‑AUC:** 0.883  
- **Balanced ROC‑AUC:** 0.889  

### **Confusion Matrix**
- True PD detected: **39 / 44**  
- True healthy detected: **11 / 15**  
- False negatives: **5** (low — good for screening)  

### **Interpretation**
The model reliably identifies PD‑related dysphonia patterns, especially:

- jitter instability  
- shimmer amplitude variation  
- reduced harmonic‑to‑noise ratio  

These biomarkers dominate PCA variance and logistic regression coefficients.

---

## **Limitations**
- Small dataset (195 samples)  
- Multiple samples per subject → potential dependence  
- Controlled recording conditions  
- No external validation  
- Linear models may miss nonlinear acoustic dynamics  

---

## **Future Improvements**
- Larger, more diverse datasets  
- Group‑aware cross‑validation  
- Non‑linear models (Random Forests, SVM, neural networks)  
- SHAP/LIME for interpretable deep learning  
- Longitudinal voice monitoring for disease progression  

---

## **Project Structure**
```
parkinsons-ml-binf760/
│
├── README.md
├── notebooks/
    └── parkinsons_voice_ml.ipynb
