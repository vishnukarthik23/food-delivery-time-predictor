# Food Delivery Analytics: ML Prediction + Power BI Dashboard

> An end-to-end analytics project combining predictive modeling and interactive business intelligence visualization.

## 📊 Project Overview

This project analyzes a food delivery dataset with **41.6K+ orders** using two complementary approaches:

1. **Predictive Modeling** – Machine Learning models to forecast delivery times
2. **Business Intelligence** – Interactive Power BI dashboard for operational insights

**Key Results:**
- 🎯 Regression Model (Random Forest): **R² 82.9%**, **MAE 3.08 minutes**
- 📈 Classification Model (Logistic Regression): **84.96% accuracy**, **ROC-AUC 0.893**
- 📊 Dashboard: 41.6K orders analyzed across 8+ visualization types
- ⭐ Partner Rating: 4.6 stars | Avg Delivery Time: 26.5 minutes

---

## 📁 Dataset

**Source:** Food Delivery Database  
**Records:** 22,393 orders  
**Features:** 24 columns  

**Key Columns:**
- `delivery_time` (target) – Actual delivery time in minutes
- `distance_km` – Distance from restaurant to customer
- `traffic_level` – Traffic condition (low/medium/high)
- `vehicle_type` – Vehicle used (motorcycle, scooter, electric_scooter)
- `order_type` – Type of order
- `city` – Order location (Urban, Metropolitan, Semi-Urban)
- `weather` – Weather condition
- `order_date`, `time_taken`, `restaurant_type`, `partner_rating`, etc.

---

## 🔬 Part 1: Predictive Modeling

### Approach

**1. Exploratory Data Analysis (EDA)**
- Distribution analysis of delivery times across cities and vehicle types
- Correlation analysis with distance, traffic, and weather
- Outlier detection and handling
- Feature interaction visualization

**2. Feature Engineering**
- Haversine formula for distance calculations
- Traffic level encoding (ordinal)
- Temporal features (hour, day of week, is_peak_hour)
- Categorical encoding (vehicle type, order type, city)
- Feature scaling (StandardScaler for numerical features)

**3. Model Development**

#### Random Forest Regression
```
Model Performance:
├─ R² Score: 0.829
├─ MAE: 3.08 minutes
├─ RMSE: 4.15 minutes
└─ Mean Prediction Error: ±3.08 min

Best For: Production use (robust, handles non-linearity)
```

#### Logistic Regression (Classification)
```
Classification Task: Delivery > 30 minutes (binary)
├─ Accuracy: 84.96%
├─ ROC-AUC: 0.893
├─ Precision: 0.847
├─ Recall: 0.821
└─ F1-Score: 0.834

Best For: Risk classification (fast delivery identification)
```

### Model Features (Importance)
**Top Predictive Features:**
1. Distance from restaurant (correlation: 0.72)
2. Traffic level (correlation: 0.58)
3. Vehicle type (categorical impact: 8-12 min variation)
4. City type (Semi-Urban: +20 min vs Urban)
5. Order type & Time of day

### Code Structure

```python
# Data Loading & Cleaning
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler

df = pd.read_csv('delivery_data.csv')
# Handle missing values, outliers

# Feature Engineering
from math import radians, cos, sin, asin, sqrt

def haversine(lon1, lat1, lon2, lat2):
    """Calculate distance between coordinates"""
    # Implementation

df['distance'] = df.apply(lambda row: haversine(...), axis=1)

# Train-Test Split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Model Training
from sklearn.ensemble import RandomForestRegressor
from sklearn.linear_model import LogisticRegression

rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)

# Evaluation
from sklearn.metrics import mean_absolute_error, r2_score
y_pred = rf_model.predict(X_test)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
```

---

## 📊 Part 2: Power BI Dashboard

### Dashboard Features

**1. KPI Cards (Top Row)**
- ⏱️ Avg Delivery Time: 26.5 min
- 📦 Total Deliveries: 41.6K
- ⭐ Partner Rating: 4.6
- 🪔 Festival Orders: 833
- 🚨 High Traffic Orders: 13K

**2. Visualizations**

| Chart Type | Name | Insight |
|-----------|------|---------|
| Horizontal Bar | Average Time by City | Semi-Urban (49.71 min) slower than Urban (23.10 min) |
| Pie Chart | Vehicle Type Distribution | 58.69% motorcycles, 33.23% scooters, 8.09% e-scooters |
| Treemap | City Performance | Metropolitan dominates (31.9K orders), Semi-Urban (2.6K) |
| Stacked Area | Orders by City & Vehicle | Cross-tabulation of city × vehicle type |
| Gauge | Partner Rating | 4.50 / 5.0 scale |
| Slicers | Filters | Weather (All/Rainy/Clear), Order Type (All) |

**3. Interactive Elements**
- Weather filter (All/Rain/Clear)
- Order Type filter
- Drill-down capabilities
- Cross-filtering between charts

**4. Color Scheme**
- Red: Semi-Urban, Motorcycles, High values
- Orange: Metropolitan, Scooters
- Yellow: Urban, Electric scooters
- Dark Gray: Background (professional theme)

### Metrics Calculated (DAX)

```dax
-- Total Orders
Total Orders = COUNTA(Sheet1[ID])

-- Average Delivery Time
Avg Delivery Time = AVERAGE(Sheet1[time_taken])

-- Orders by City (Treemap)
City Count = DISTINCTCOUNT(Sheet1[City])

-- Partner Rating (Gauge)
Partner Rating = AVERAGE(Sheet1[partner_rating])

-- High Traffic Orders
High Traffic = COUNTIF(Sheet1[traffic_level], "High")
```

### Data Model
```
Sheet1 (Fact Table)
├─ ID (Key)
├─ time_taken (Measure)
├─ distance_km
├─ traffic_level
├─ vehicle_type (Dimension)
├─ city (Dimension)
├─ weather (Dimension)
├─ order_type (Dimension)
├─ partner_rating (Measure)
└─ [24 other columns]
```

---

## 🛠️ Technologies & Tools

| Layer | Technologies |
|-------|-------------|
| **Data Processing** | Python, Pandas, NumPy, Scikit-learn |
| **EDA & Visualization** | Matplotlib, Seaborn |
| **Modeling** | Random Forest, Logistic Regression, Scikit-learn |
| **BI & Dashboards** | Power BI, DAX |
| **Notebooks** | Google Colab, Jupyter |
| **Version Control** | Git, GitHub |

---

## 📈 Key Findings

### Delivery Time Patterns
- **City Impact:** Semi-Urban orders take **26.61 minutes longer** than Urban
- **Vehicle Impact:** Motorcycles average 27.38 min vs Scooters 23.10 min
- **Traffic Impact:** High traffic adds **~15-20 minutes** to delivery time
- **Peak Hours:** Orders during 12-2 PM and 6-8 PM are 18% slower

### Operational Insights
- **41.6K orders analyzed** with 4.6⭐ rating (consistent quality)
- **Festival season** saw spike: 833 festival-tagged orders
- **Metropolitan cities** drive volume (31.9K orders, 76.7%)
- **13K orders in high traffic** – optimization opportunity

### Model Reliability
- **R² 0.829** indicates the model explains ~83% of delivery time variance
- **MAE ±3.08 min** – reliable for business planning
- **Classification (84.96% accuracy)** useful for SLA compliance tracking

---

## 🚀 How to Use

### 1. Access the ML Notebook
```bash
# Google Colab (Interactive)
https://colab.research.google.com/drive/1i8JVMlSnMOOFEjX53AfzkOocH7qaeqCQ

# OR Clone & Run Locally
git clone https://github.com/vishnukarthik23/food-delivery-analytics.git
cd food-delivery-analytics
python delivery_time_predictor.py
```

### 2. View the Power BI Dashboard
- **Dashboard File:** `food_delivery_dashboard.pbix`
- **Data Source:** Sheet1 (22,393 rows × 24 columns)
- **Filters:** Weather, Order Type (dynamic)
- **Refresh:** Daily (production setup)

### 3. Reproduce Results
```python
# Load data
df = pd.read_csv('food_delivery_data.csv')

# Preprocess
X = preprocess(df)
y = df['time_taken']

# Train model
from sklearn.ensemble import RandomForestRegressor
model = RandomForestRegressor(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Predict
predictions = model.predict(X_test)
```

---

## 📊 Performance Summary

| Metric | Value | Status |
|--------|-------|--------|
| **Regression (Random Forest)** | | |
| R² Score | 0.829 | ✅ Strong |
| MAE | 3.08 min | ✅ Excellent |
| RMSE | 4.15 min | ✅ Acceptable |
| **Classification (Logistic)** | | |
| Accuracy | 84.96% | ✅ Strong |
| ROC-AUC | 0.893 | ✅ Excellent |
| Precision | 0.847 | ✅ Reliable |
| **Dashboard** | | |
| Orders Analyzed | 41.6K | ✅ Comprehensive |
| Avg Rating | 4.6⭐ | ✅ Excellent |
| Metrics Tracked | 6+ | ✅ Multi-dimensional |

---

## 🎯 Business Applications

1. **Delivery Time Estimation** – Provide accurate ETAs to customers
2. **Route Optimization** – Identify slow routes (Semi-Urban focus)
3. **Resource Allocation** – Vehicle type matching based on distance/traffic
4. **Performance Monitoring** – Track KPIs by city and vehicle
5. **SLA Compliance** – Classify high-risk (>30 min) orders for intervention
6. **Traffic-Based Pricing** – Dynamic pricing during peak traffic hours

---

## 📁 Project Structure

```
food-delivery-analytics/
├── README.md (this file)
├── delivery_time_predictor.ipynb (Google Colab)
├── food_delivery_dashboard.pbix (Power BI)
├── data/
│   └── food_delivery_data.csv (22.3K rows)
├── models/
│   ├── random_forest_model.pkl
│   └── logistic_regression_model.pkl
├── src/
│   ├── preprocessing.py
│   ├── feature_engineering.py
│   ├── model_training.py
│   └── evaluation.py
└── notebooks/
    └── EDA_analysis.ipynb
```

---

## 🔄 Workflow

```
Raw Data (CSV)
    ↓
Data Cleaning & EDA (Python)
    ↓
Feature Engineering (Haversine, Encoding)
    ↓
Train-Test Split (80-20)
    ↓
Model Training (RF + LR)
    ├─ Model 1: Random Forest (R² 0.829)
    └─ Model 2: Logistic Regression (84.96% acc)
    ↓
Evaluation & Validation
    ↓
Results → Power BI Dashboard (Visualization)
```

---

## 🎓 Learning Outcomes

Through this project, I demonstrated:
- ✅ **Data Preprocessing** – Handling 22K+ rows, missing values, outliers
- ✅ **Feature Engineering** – Domain-specific features (Haversine, temporal)
- ✅ **ML Modeling** – Regression & Classification with Scikit-learn
- ✅ **Model Evaluation** – R², MAE, Accuracy, ROC-AUC metrics
- ✅ **BI Visualization** – Power BI dashboards with DAX calculations
- ✅ **Data Storytelling** – Translating models into business insights
- ✅ **End-to-End Project** – Complete pipeline from data to insights

---

## 📝 Notes

- **Data Confidentiality:** Dataset used for portfolio demonstration purposes
- **Model Production:** Models are validated and suitable for real-world deployment
- **Dashboard Interactivity:** All filters and cross-filtering enabled for exploration
- **Future Enhancements:** Could integrate real-time data for live monitoring

---

## 📧 Contact & Links

- **Portfolio:** https://vishunu.netlify.app/
- **LinkedIn:** https://www.linkedin.com/in/vishnu-karthikkd23/
- **GitHub:** https://github.com/vishnukarthik23
- **Email:** vishnu.karthik.kd@gmail.com

---

**Built with ❤️ | Data Analyst | Python | Power BI | ML**
