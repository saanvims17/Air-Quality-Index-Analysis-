# Air-Quality-Index-Analysis-

**Project Overview**
*This project analyzes and predicts Air Quality Index (AQI) using machine learning.*
It employs:
*Linear Regression* – To predict exact AQI values.
*Random Forest Classification* – To classify AQI into categories (Good, Moderate, Poor, etc.).
Dataset

 **Why the Project Is Useful?**
-> Environmental Monitoring: Helps in tracking pollution levels.
-> Health Awareness: Identifies areas with unsafe air quality.
-> Policy Making: Assists governments in taking preventive measures.
-> Predictive Analysis: Allows forecasting of AQI trends for better planning.

**The dataset includes air quality measurements with features such as:**
-> Pollutants: PM2.5, PM10, NO2, SO2, O3
-> AQI: Numeric target for regression
-> AQI_Bucket: Categorical target for classification

**Technologies Used**
-> Python
-> Pandas, NumPy – Data handling
-> Matplotlib, Seaborn – Data visualization
-> Scikit-learn – Machine learning models

**Project Workflow**
**Data Preprocessing**
Handled missing values using mean imputation.
Encoded AQI categories into numerical labels.
Split data into training and testing sets.

**Model 1: Linear Regression (AQI Prediction)**
Features: PM2.5, PM10, NO2, SO2, O3
Target: AQI
**Performance:**
R² Score: 0.935 (93.5% accuracy)
Mean Absolute Error: 16.25
Root Mean Squared Error: 22.53

**Model 2: Random Forest Classification (AQI Category Prediction)**
Features: PM2.5, PM10, NO2, SO2, O3
Target: AQI_Bucket
**Performance:**
Accuracy: 88.5%
Classification Report: Strong precision and recall across AQI categories
