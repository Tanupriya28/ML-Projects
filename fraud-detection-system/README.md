# AI Financial Fraud Detection System

An end-to-end machine learning system for detecting fraudulent financial transactions using behavioral transaction patterns.

The project uses the **PaySim financial transaction dataset (6.36M transactions)** and deploys the trained model through a **Flask API with an interactive dashboard**.

The system predicts the **probability of fraud** for a transaction and provides a **risk decision (Approve / Manual Review / Block)** along with **model explainability using SHAP**.

---

# Project Overview

Financial fraud detection is a critical problem in digital payments and banking systems. Fraudulent transactions represent a very small fraction of total transactions, making the problem highly **imbalanced and difficult to detect**.

This project builds a **machine learning pipeline** that:

1. Analyzes transaction behavior
2. Identifies suspicious transaction patterns
3. Predicts fraud probability
4. Provides explainable insights into the prediction
5. Deploys the model through an interactive web application

---

# Dataset

Dataset used: **PaySim Financial Transaction Dataset**

Total transactions: **6,362,620**

Fraud transactions: **8,213**

Fraud percentage:  **0.13%**

The dataset simulates real mobile money transactions.

### Key Features

| Feature | Description |
|------|------|
step | time step of transaction |
type | transaction type |
amount | transaction amount |
oldbalanceOrg | sender balance before transaction |
newbalanceOrig | sender balance after transaction |
oldbalanceDest | receiver balance before transaction |
newbalanceDest | receiver balance after transaction |
isFraud | fraud label |

---

# Exploratory Data Analysis

Important insights discovered during analysis:

• Fraud occurs mainly in **TRANSFER** and **CASH_OUT** transactions  
• Fraudulent transactions often involve **large transaction amounts**  
• Fraud cases frequently show **balance inconsistencies**  
• Dataset is **extremely imbalanced (0.13% fraud)**  

These patterns informed feature engineering and model selection.

---

# Feature Engineering

Additional behavioral features were created to improve detection.

### Balance Error Features

errorBalanceOrig = oldbalanceOrg − amount − newbalanceOrig

errorBalanceDest = oldbalanceDest + amount − newbalanceDest

These features capture **balance inconsistencies**, which are strong indicators of fraud.

### One-Hot Encoding

Transaction types were converted to numerical features:

type_TRANSFER  
type_CASH_OUT  
type_PAYMENT  
type_DEBIT  

---

# Machine Learning Models

Three models were trained and evaluated:

| Model | Purpose |
|------|------|
Random Forest | baseline model |
XGBoost | optimized fraud detection |
LightGBM | additional comparison |

Train/Test Split:

80% training  
20% testing  

Training transactions ≈ **5.08M**  
Testing transactions ≈ **1.27M**

---

# Model Performance

### Random Forest Results

Confusion Matrix:

True Negatives: 1,270,904  
True Positives: 1,616  
False Negatives: 4  
False Positives: 0  

Fraud Detection Metrics:

Precision: **1.00**  
Recall: **0.997**  
F1 Score: **0.998**

---

### XGBoost Results (Final Model)

Confusion Matrix:

True Negatives: 1,270,844  
True Positives: 1,616  
False Negatives: 4  
False Positives: 60  

Fraud Detection Metrics:

Precision: **0.96**  
Recall: **0.998**  
F1 Score: **0.98**

XGBoost achieved the best **precision-recall balance**, making it the final selected model.

---

### LightGBM Results

Precision: **0.03**  
Recall: **0.97**

Although recall was high, precision was extremely low, meaning too many normal transactions were incorrectly flagged as fraud.

---

# Model Explainability (SHAP)

To improve transparency, **SHAP (SHapley Additive Explanations)** was implemented.

Top features influencing fraud predictions:

• errorBalanceOrig  
• transaction amount  
• sender balance before transaction  
• transaction type (TRANSFER)

This allows the system to explain **why a transaction was flagged as suspicious**.

---

# Fraud Risk Scoring System

Instead of a binary fraud prediction, the system generates a **fraud probability score**.

Example output:

Fraud Probability: 0.82  
Decision: BLOCK TRANSACTION  
Risk Level: HIGH

Decision logic:

| Probability | Action |
|------|------|
< 0.30 | Approve |
0.30 – 0.70 | Manual Review |
> 0.70 | Block Transaction |

This mimics real fraud monitoring systems used by fintech platforms.

---

# System Architecture

User Transaction Input  
↓  
Flask Web Interface  
↓  
Flask API Endpoint  
↓  
Feature Processing  
↓  
XGBoost Fraud Detection Model  
↓  
Fraud Probability Score  
↓  
Decision Engine  
↓  
Risk Visualization + SHAP Explanation

---

# Web Dashboard

The application includes a simple interactive dashboard where users can enter transaction details.

Users receive:

• Fraud probability score  
• Risk level (Low / Medium / High)  
• Decision recommendation  
• Visual risk meter  
• Top risk factors (SHAP explainability)

---

# API Usage

### Endpoint

POST /predict

### Example Request
{
 "step":1,
 "amount":50000,
 "oldbalanceOrg":60000,

 Example Response
{
 "fraud_probability": 0.00038,
 "decision": "APPROVE",
 "risk_level": "LOW"
}

## Project Structure
fraud-detection-system
│
├ app
│   └ app.py
│
├ models
│   ├ fraud_model_xgb.pkl
│   └ feature_columns.pkl
│
├ templates
│   └ index.html
│
├ images
│   ├ dashboard.png
│   ├ result.png
│   ├ api_test.png
│   └ shap_features.png

│
├ requirements.txt
└ README.md

## Installation

### Clone repository:

git clone https://github.com/yourusername/fraud-detection-system.git

### Navigate to project:

cd fraud-detection-system

### Create virtual environment:

python -m venv venv

Activate environment:

### Windows:

venv\Scripts\activate

### Install dependencies:

pip install -r requirements.txt

Run application:

python app/app.py

### Open browser:

http://127.0.0.1:5000


## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- LightGBM
- SHAP
- Flask
- HTML / CSS

## Key Skills Demonstrated

- Machine Learning
- Fraud Detection Modeling
- Feature Engineering
- Handling Imbalanced Data
- Model Explainability (SHAP)
- Model Deployment (Flask API)
- Interactive ML Dashboards

## Future Improvements

- Real-time streaming fraud detection
- Integration with cloud deployment
- Advanced anomaly detection models
- Live transaction monitoring dashboards
