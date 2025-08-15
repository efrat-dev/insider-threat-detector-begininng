# מבנה הפרויקט המומלץ

```
project_root/
│
├─ data/
│   └─ raw/                         # OUTPUT של data_generation
│
├─ 01_data_generation_pipeline/     
├─ 02_eda.ipynb
├─ 03_preprocessing_pipeline/       
├─ 04_modeling_pipeline/            
│   ├─ 04a_lstm.ipynb
│   ├─ 04b_isolation_forest.ipynb
│   └─ 04c_xgboost.ipynb           # כאן נוצרים train/val/test של XGBoost
│
├─ outputs/                         
│   ├─ data_generation/             # raw data 
│   ├─ preprocessing/               # train/val/test ל-LSTM ו-Isolation Forest
│   ├─ lstm/                        # scores + model artifacts
│   ├─ isolation_forest/            # scores   
│   └─ xgboost/                     # train/val/test מועשרים + predictions
│
└─ scripts/                         
```

## זרימת הנתונים:

### שלב 1: יצירת נתונים
`01_data_generation_pipeline/` → `data/raw/`

### שלב 2: עיבוד מקדים  
`03_preprocessing_pipeline/` קורא מ-`data/raw/` → `outputs/preprocessing/`
- יוצר: train/val/test ל-LSTM ו-Isolation Forest

### שלב 3: מודלים ראשוניים
- **LSTM**: קורא מ-`outputs/preprocessing/` → `outputs/lstm/` (scores)
- **Isolation Forest**: קורא מ-`outputs/preprocessing/` → `outputs/isolation_forest/` (scores)

### שלב 4: XGBoost (Ensemble)
`04c_xgboost.ipynb`:
1. קורא מ-`outputs/preprocessing/` (הנתונים המקוריים)
2. קורא מ-`outputs/lstm/` ו-`outputs/isolation_forest/` (הציונים)  
3. **יוצר בעצמו** train/val/test חדשים עם הציונים → `outputs/xgboost/`
4. מריץ את המודל → predictions ב-`outputs/xgboost/`

## יתרונות המבנה:
- **זרימה ליניארית וברורה** של כל שלב
- **אין כפילויות** - כל output נמצא במקום הנכון
- **קל למעקב** - ברור מאיפה כל קובץ מגיע
- **גמישות** - קל להוסיף מודלים נוספים לעתיד

- # מבנה הפרויקט המומלץ - רמה מפורטת

```
project_root/
│
├─ data/
│   └─ raw/
│       ├─ employees.csv            # נתוני עובדים
│       ├─ actions.csv              # נתוני פעולות
│       └─ metadata.json            # מטא-דאטה על היצירה
│
├─ 01_data_generation_pipeline/
│   ├─ generate_employees.py
│   ├─ generate_actions.py
│   ├─ config.yaml
│   └─ utils.py
│
├─ 02_eda.ipynb
│
├─ 03_preprocessing_pipeline/
│   ├─ feature_engineering.py
│   ├─ data_splitting.py
│   ├─ config.yaml
│   └─ utils.py
│
├─ 04_modeling_pipeline/
│   ├─ 04a_lstm.ipynb
│   ├─ 04b_isolation_forest.ipynb
│   ├─ 04c_xgboost.ipynb
│   └─ utils/
│       ├─ model_utils.py
│       ├─ evaluation_utils.py
│       └─ plotting_utils.py
│
├─ outputs/
│   ├─ data_generation/
│   │   ├─ employees_generated.csv
│   │   ├─ actions_generated.csv
│   │   ├─ generation_stats.json
│   │   └─ logs/
│   │       └─ generation_YYYY-MM-DD.log
│   │
│   ├─ preprocessing/
│   │   ├─ lstm_isolation/
│   │   │   ├─ train.csv
│   │   │   ├─ val.csv
│   │   │   ├─ test.csv
│   │   │   └─ feature_stats.json
│   │   └─ logs/
│   │       └─ preprocessing_YYYY-MM-DD.log
│   │
│   ├─ lstm/
│   │   ├─ models/
│   │   │   ├─ lstm_model.h5
│   │   │   ├─ lstm_weights.h5
│   │   │   └─ model_architecture.json
│   │   ├─ predictions/
│   │   │   ├─ train_predictions.csv
│   │   │   ├─ val_predictions.csv
│   │   │   ├─ test_predictions.csv
│   │   │   └─ employee_scores.csv      # ציונים ברמת עובד
│   │   ├─ metrics/
│   │   │   ├─ training_history.json
│   │   │   ├─ evaluation_metrics.json
│   │   │   └─ confusion_matrix.png
│   │   └─ logs/
│   │       └─ lstm_training_YYYY-MM-DD.log
│   │
│   ├─ isolation_forest/
│   │   ├─ models/
│   │   │   ├─ isolation_forest_model.pkl
│   │   │   └─ model_params.json
│   │   ├─ predictions/
│   │   │   ├─ train_predictions.csv
│   │   │   ├─ val_predictions.csv
│   │   │   ├─ test_predictions.csv
│   │   │   └─ employee_scores.csv      # ציונים ברמת עובד
│   │   ├─ metrics/
│   │   │   ├─ evaluation_metrics.json
│   │   │   ├─ feature_importance.json
│   │   │   └─ outlier_analysis.png
│   │   └─ logs/
│   │       └─ isolation_forest_YYYY-MM-DD.log
│   │
│   └─ xgboost/
│       ├─ data/                    # הנתונים המועשרים
│       │   ├─ xgboost_train.csv
│       │   ├─ xgboost_val.csv
│       │   ├─ xgboost_test.csv
│       │   └─ feature_engineering_log.json
│       ├─ models/
│       │   ├─ xgboost_model.json
│       │   ├─ xgboost_model.pkl
│       │   └─ model_params.json
│       ├─ predictions/
│       │   ├─ final_predictions.csv
│       │   ├─ prediction_probabilities.csv
│       │   └─ employee_risk_scores.csv
│       ├─ metrics/
│       │   ├─ evaluation_metrics.json
│       │   ├─ feature_importance.json
│       │   ├─ shap_analysis.json
│       │   ├─ roc_curve.png
│       │   └─ feature_importance.png
│       └─ logs/
│           └─ xgboost_training_YYYY-MM-DD.log
│
├─ scripts/
│   ├─ run_full_pipeline.py
│   ├─ evaluate_models.py
│   ├─ generate_report.py
│   └─ utils/
│       ├─ file_utils.py
│       ├─ logging_utils.py
│       └─ config_utils.py
│
├─ config/
│   ├─ main_config.yaml
│   ├─ model_configs.yaml
│   └─ paths_config.yaml
│
├─ docs/
│   ├─ README.md
│   ├─ data_dictionary.md
│   ├─ model_documentation.md
│   └─ api_documentation.md
│
├─ requirements.txt
├─ environment.yml
└─ .gitignore
```

## הסברים לתתי-תיקיות:

### תיקיות models/
שומרות את המודלים המאומנים בפורמטים שונים (h5, pkl, json)

### תיקיות predictions/  
תוצאות החיזוי של כל מודל לכל סט (train/val/test) + ציונים ברמת עובד

### תיקיות metrics/
מדדי הערכה, גרפים, וניתוחי ביצועים של כל מודל

### תיקיות logs/
לוגים מפורטים של כל ריצה עם חותמת זמן

### תיקיית config/
קבצי הגדרות מרכזיים לכל הפרויקט

### תיקיית docs/
תיעוד מפורט של הפרויקט והמודלים

### נקודות מיוחדות:

#### XGBoost data/
הנתונים המועשרים עם ציונים של LSTM ו-Isolation Forest

#### employee_scores.csv
ציונים מצרפים ברמת עובד (לא ברמת פעולה) - חשוב לintegration

#### שמות קבצים עם תאריכים
מאפשר לעקוב אחר ריצות שונות ולשמור היסטוריה

# כך ChatGPT כתב (הגרסה הלא מפורטת)

project_root/
│
├─ data/
│   └─ raw/                         # raw שנוצר מ-data_generation
│
├─ 01_data_generation_pipeline/     # סאבמודול / פרויקט נפרד
├─ 02_eda.ipynb
├─ 03_preprocessing_pipeline/       # סאבמודול / פרויקט נפרד
├─ 04_modeling_pipeline/            # notebooks של המודלים
│   ├─ 04a_lstm.ipynb
│   ├─ 04b_isolation_forest.ipynb
│   └─ 04c_xgboost.ipynb
│
├─ outputs/                         # כל התוצאות של השלבים
│   ├─ data_generation/             # raw שנוצר
│   ├─ preprocessing/               # train/val/test ל-LSTM ו-Isolation Forest
│   ├─ lstm/                        # predictions של LSTM
│   ├─ isolation_forest/            # predictions של Isolation Forest
│   └─ xgboost/                     # train/val/test + predictions של XGBoost
│
└─ scripts/ 
