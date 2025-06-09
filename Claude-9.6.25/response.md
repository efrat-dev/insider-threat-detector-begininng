# 🛡️ Insider Threat Synthetic Data Generator

I’ve built you a **comprehensive synthetic data generation system** for insider threat detection!  
The system includes:

---

## 🎯 Realistic Data Generator

- 1,000 employees with diverse profiles (departments, campuses, roles)  
- 7% malicious insiders (as requested)  
- 180 days of activity per employee  
- Complex and realistic behavioral patterns  

---

## 📊 Tracked Activities

- **Printing**: volume, time of day, color usage, multiple campuses  
- **Burning**: CD/USB activity, classification levels, volumes  
- **Travel**: international travel, hostile countries, suspicious private trips  
- **Facility Access**: times, number of campuses, off-hour entries  

---

## 🤖 Full ML Pipeline

- Trains 3 models: **Random Forest**, **Gradient Boosting**, **Logistic Regression**  
- Advanced feature preparation: encoding, feature engineering  
- Model evaluation: **ROC curves**, **confusion matrices**, **feature importance**  

---

## 📈 Comprehensive EDA (Exploratory Data Analysis)

- Distributions by department, role  
- Behavioral patterns: malicious vs. normal users  
- Correlation matrices  
- Detailed visualizations  

---

## 💻 How to Run

```python
# To generate 50,000 records:
dataset, model_results = main(50000)

# To generate other volumes:
dataset_10k, results = main(10000)   # 10K records
dataset_100k, results = main(100000) # 100K records
```

## 🎭 Realistic Malicious Patterns

- Printing during unusual hours  
- Large-volume burning activity  
- Accessing materials beyond classification level  
- Activity in multiple campuses  
- Private trips to hostile countries **combined with simultaneous activity**

---

## 🔍 Smart Risk Indicators

- `risk_travel_indicator`: detects suspicious behavior during private travel  
- `campus_mismatch`: activity in a campus different from the registered one  
- `off_hours_ratios`: ratio of activity outside normal working hours  

---

## 📊 High Model Performance

- The generated data includes **complex behavior patterns** for models to learn from—**not too easy**!  
- The system saves the data as CSV and produces **detailed visualizations**