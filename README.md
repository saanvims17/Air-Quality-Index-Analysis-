# Air-Quality-Index-Analysis-

**Project Overview**

*This project analyzes and predicts Air Quality Index (AQI) using machine learning.*
It employs:

**Linear Regression** – To predict exact AQI values.

**Random Forest Classification** – To classify AQI into categories (Good, Moderate, Poor, etc.).
Dataset

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

-> Handled missing values using mean imputation.

-> Encoded AQI categories into numerical labels.

-> Split data into training and testing sets.

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

**How to Get Started**

1. Clone the repository from GitHub.

*Example:* Download the project files to your local system.

2. Navigate to the project folder on your local system.

3. Install the required dependencies for running the project.

*Example:* Install necessary libraries like Pandas, NumPy, and Scikit-learn.

4. Run the script to analyze and predict air quality.

*Example:* Execute the Python file to process the dataset and train models.

5. View the output and visualizations to interpret AQI trends.

Example: Graphs showing air pollution levels over time will be displayed.


 **Why the Project Is Useful?**
 
-> Environmental Monitoring: Helps in tracking pollution levels.

-> Health Awareness: Identifies areas with unsafe air quality.

-> Policy Making: Assists governments in taking preventive measures.

-> Predictive Analysis: Allows forecasting of AQI trends for better planning.

