# Heart Disease Prediction using Machine Learning
---

## 🎯 Project Overview

Heart disease is one of the leading causes of death worldwide, and early detection can save lives. This project builds and compares several classic machine learning classifiers to predict the presence of heart disease from routine clinical and test data such as age, cholesterol, resting blood pressure, chest pain type and ECG results.

**Goals**

- Clean and validate a real-world medical dataset (including hidden errors).
- Explore the data visually to understand which factors relate to heart disease.
- Build a **leak-free preprocessing pipeline** with scikit-learn.
- Train and fairly compare **Naive Bayes, Decision Tree, K-Nearest Neighbours (KNN) and Logistic Regression**.
- Evaluate models with metrics that matter in medicine, especially the **False Negative Rate (FNR)**.

---

## 🏆 Key Results

| Metric | Value |
|---|---|
| Best model (by cross-validation) | **Logistic Regression** |
| 5-fold CV accuracy | **85.77% (± 3.23)** |
| Test-set accuracy | **88.52%** |
| False Negative Rate (FNR) | **17.86%** |
| ROC AUC (test set) | **0.953** |

Logistic Regression gave the most reliable performance across the different data splits, even though several models tied on test accuracy. On this small dataset (only 61 test patients), cross-validation is a more trustworthy way to compare models than a single test split.

---

## 📊 Dataset

- **File:** `heart.csv`
- **Size:** 303 rows × 14 columns (302 rows after removing one duplicate)
- **Type:** Binary classification (heart disease: yes / no)
- **Origin:** The widely used Heart Disease dataset (Cleveland data from the UCI Machine Learning Repository, also distributed on Kaggle).

### Features

The original short column names are renamed in the notebook to be easier to read.

| Original name | Renamed to | Description | Values |
|---|---|---|---|
| `age` | `age` | Age in years | Numeric (29 to 77) |
| `sex` | `sex` | Sex | 0 = female, 1 = male |
| `cp` | `chest_pain_type` | Chest pain type | 0 = asymptomatic, 1 = atypical angina, 2 = non-anginal pain, 3 = typical angina |
| `trestbps` | `resting_blood_pressure` | Resting blood pressure (mm Hg) | Numeric |
| `chol` | `cholesterol` | Serum cholesterol (mg/dl) | Numeric |
| `fbs` | `fasting_blood_sugar` | Fasting blood sugar > 120 mg/dl | 0 = no, 1 = yes |
| `restecg` | `rest_ecg` | Resting ECG result | 0 = left ventricular hypertrophy, 1 = normal, 2 = ST-T wave abnormality |
| `thalach` | `max_heart_rate_achieved` | Maximum heart rate achieved | Numeric |
| `exang` | `exercise_induced_angina` | Exercise-induced angina | 0 = no, 1 = yes |
| `oldpeak` | `st_depression` | ST depression induced by exercise relative to rest | Numeric |
| `slope` | `st_slope` | Slope of the peak exercise ST segment | 0 = downsloping, 1 = flat, 2 = upsloping |
| `ca` | `num_major_vessels` | Number of major vessels colored by fluoroscopy | 0 to 3 |
| `thal` | `thalassemia` | Thalassemia blood disorder result | 1 = fixed defect, 2 = normal, 3 = reversible defect |
| `target` | `target` | **Label:** heart disease | 0 = no disease, 1 = disease (after correction, see below) |

### Class balance (after cleaning)

| Class | Patients | Share |
|---|---|---|
| No disease | 164 | 54.3% |
| Disease | 138 | 45.7% |

The classes are reasonably balanced. About 68% of the patients are male and 32% female.
### Observations from the data

Comparing the average patient with and without heart disease:

| Feature | No disease | Disease |
|---|---|---|
| Average age | 52.6 | 56.6 |
| Max heart rate achieved | 158.4 | 139.1 |
| Exercise-induced angina (share) | 0.14 | 0.55 |
| ST depression | 0.59 | 1.59 |
| Number of major vessels | 0.27 | 1.15 |
| Share of male patients | 0.56 | 0.83 |

Patients with heart disease tend to be older, reach a lower maximum heart rate, are more likely to have exercise-induced angina, show higher ST depression and have more blocked major vessels. These are descriptive patterns in this dataset, not proof of cause.

---

## 🤖 Models Used

All models are wrapped in the same scikit-learn `Pipeline`, so they are tested in exactly the same way.

| Model | Notes |
|---|---|
| **Gaussian Naive Bayes** | Fast probabilistic baseline |
| **Decision Tree** | `criterion="entropy"`, `max_depth=3`, easy to visualise and interpret |
| **KNN (k = 7)** | Distance-based classifier |
| **KNN (k = 4)** | Included to show how the choice of *k* changes results |
| **KNN (best k = 17)** | *k* chosen by cross-validation on the **training** data only |
| **Logistic Regression** | `max_iter=1000`, linear model that outputs probabilities |

### Preprocessing pipeline

| Column type | Columns | Steps |
|---|---|---|
| Numeric | `age`, `resting_blood_pressure`, `cholesterol`, `max_heart_rate_achieved`, `st_depression`, `num_major_vessels` | Median imputation → Standard scaling |
| Categorical | `sex`, `chest_pain_type`, `fasting_blood_sugar`, `rest_ecg`, `exercise_induced_angina`, `st_slope`, `thalassemia` | Most-frequent imputation → One-hot encoding |

One-hot encoding stops the model from wrongly treating a code like `3` as "bigger" than `1`. Scaling matters especially for KNN and Logistic Regression.

### Train/test split

- 80% training (241 patients) and 20% testing (61 patients)
- `stratify=y` keeps the same disease share in both parts
- `random_state=123` for reproducible results


## 📈 Results in Detail

Models are sorted by cross-validation accuracy:

| Rank | Model | Test accuracy | FNR | CV accuracy | CV std | ROC AUC |
|---|---|---|---|---|---|---|
| 1 | **Logistic Regression** | 88.52% | **17.86%** | **85.77%** | 3.23 | **0.953** |
| 2 | KNN (k = 7) | 88.52% | 21.43% | 82.79% | 4.23 | 0.938 |
| 3 | KNN (best k = 17) | 88.52% | 21.43% | 82.44% | 3.60 | 0.943 |
| 4 | Naive Bayes | 88.52% | 21.43% | 82.44% | 4.43 | 0.951 |
| 5 | Decision Tree | 88.52% | 17.86% | 80.50% | 5.26 | 0.909 |
| 6 | KNN (k = 4) | 86.89% | 25.00% | 80.48% | 2.75 | 0.920 |

### Takeaways

- Almost all models reach the same test accuracy (88.52%), so the test set alone cannot separate them. Cross-validation reveals the real differences.
- **Logistic Regression** is best on cross-validation accuracy and ROC AUC, and ties for the lowest FNR.
- The shallow **Decision Tree** has the highest variance between folds (± 5.26) and the lowest AUC.
- A poorly chosen *k* hurts KNN (k = 4 has the highest FNR).
- Differences between several models are small relative to the standard deviation, so treat the ranking with caution on a dataset this small.

---
## ⚙️ Installation and Usage

### 1. Clone the repository

```bash
git clone https://github.com/Ash19-23/Heart_Disease.git
cd Heart_Disease
```

### 2. (Recommended) Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter

Or save the following as `requirements.txt` and run `pip install -r requirements.txt`:

```
numpy
pandas
matplotlib
seaborn
scikit-learn>=1.0
jupyter
```

### 4. Add the dataset

Download `heart.csv` (Heart Disease dataset from Kaggle(https://www.kaggle.com/datasets/ritwikb3/heart-disease-cleveland))

### 5. Run the notebook

```bash
jupyter notebook Heart_Disease.ipynb
```

Then choose **Kernel → Restart & Run All**. You can also open the notebook in **Google Colab** by uploading `heart.csv` to the session.
```
## ⚠️ Limitations

- **Small dataset:** Only 302 patients, with a 61-patient test set. Results can change noticeably with a different split, which is why cross-validation is reported.
- **Single source:** The data comes from one hospital study, so the model may not generalise to other populations.
- **Sex imbalance:** About two thirds of the patients are male.
- **No hyperparameter tuning** beyond choosing *k* for KNN.
- **Not clinically validated:** The model has not been tested on real clinical workflows and must not be used for medical decisions.

---

## 🚀 Future Improvements

- Add more models such as Random Forest, Gradient Boosting, XGBoost and SVM
- Tune hyperparameters with `GridSearchCV` or `RandomizedSearchCV`
- Add feature-importance and model-explainability analysis (e.g. SHAP)
- Optimise the decision threshold to further reduce the false negative rate
- Validate on an additional, independent heart disease dataset
- Package the best model into a simple web app (Streamlit or Flask) for demonstration
- Save the trained pipeline with `joblib` for reuse

---
