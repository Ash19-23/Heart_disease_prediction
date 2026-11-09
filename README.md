About the dataset (column meanings)

This file is a copy of the well-known Cleveland heart-disease data. Important: the number codes in this CSV are different from the codes written in many tutorials (including the original version of this notebook). Section 5 proves this with evidence, and the table below already uses the correct codes for this file.
column_info = {
    "age":      "age in years",
    "sex":      "1 = male, 0 = female",
    "cp":       "chest pain type: 0 = asymptomatic, 1 = atypical angina, 2 = non-anginal pain, 3 = typical angina",
    "trestbps": "resting blood pressure (mm Hg)",
    "chol":     "serum cholesterol (mg/dl)",
    "fbs":      "fasting blood sugar > 120 mg/dl (1 = yes, 0 = no)",
    "restecg":  "resting ECG: 0 = left ventricular hypertrophy, 1 = normal, 2 = ST-T wave abnormality",
    "thalach":  "maximum hea…   "exang":    "exercise-induced angina (1 = yes, 0 = no)",
    "oldpeak":  "ST depression induced by exercise relative to rest",
    "slope":    "slope of the peak exercise ST segment: 0 = downsloping, 1 = flat, 2 = upsloping",
    "ca":       "number of major vessels (0-3) coloured by fluoroscopy",
    "thal":     "thalassemia: 1 = fixed defect, 2 = normal, 3 = reversible defect",
    "target":   "in this CSV: 1 = NO heart disease, 0 = heart disease (see Section 5; we flip it so 1 = disease)",
