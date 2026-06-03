# 🛡️ Network Security — Phishing URL Detection System

An end-to-end machine learning pipeline that detects phishing URLs using a modular MLOps architecture. Built with FastAPI, MongoDB, MLflow, and DagsHub — covering the full lifecycle from data ingestion to model deployment.

## Overview

Phishing attacks remain one of the most common cybersecurity threats. This project tackles the problem by training a classification model on 30 URL-based features to distinguish phishing URLs from legitimate ones. The system is production-ready with a modular pipeline, drift detection, experiment tracking, and a REST API for real-time predictions.

## Features

- Upload a CSV of URLs and get phishing predictions instantly
- End-to-end ML pipeline: ingestion → validation → transformation → training
- Data drift detection using the KS test
- Experiment tracking with MLflow and DagsHub
- Model selection via cross-validated hyperparameter tuning across 5 classifiers
- FastAPI backend with `/train` and `/predict` endpoints
- Dockerized for consistent deployment

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | FastAPI, Python |
| Database | MongoDB Atlas |
| ML Framework | Scikit-learn |
| Experiment Tracking | MLflow, DagsHub |
| Data Validation | SciPy (KS Test) |
| Preprocessing | KNN Imputer, Scikit-learn Pipeline |
| Containerization | Docker |
| CI/CD | GitHub Actions |

## ML Pipeline

```
MongoDB Atlas
     ↓
Data Ingestion      → pulls phishing dataset, splits into train/test
     ↓
Data Validation     → schema check + KS-test drift detection
     ↓
Data Transformation → KNN imputation, feature scaling
     ↓
Model Training      → trains 5 models, selects best via GridSearchCV
     ↓
FastAPI /predict    → serves predictions on new URL CSVs
```

## Models Evaluated

- Random Forest
- Decision Tree
- Gradient Boosting
- Logistic Regression
- AdaBoost

The best performing model is automatically selected and saved to `final_model/`.

## Features Used (30 URL-based signals)

`having_IP_Address`, `URL_Length`, `Shortining_Service`, `having_At_Symbol`, `double_slash_redirecting`, `Prefix_Suffix`, `having_Sub_Domain`, `SSLfinal_State`, `Domain_registeration_length`, `Favicon`, `port`, `HTTPS_token`, `Request_URL`, `URL_of_Anchor`, `Links_in_tags`, `SFH`, `Submitting_to_email`, `Abnormal_URL`, `Redirect`, `on_mouseover`, `RightClick`, `popUpWidnow`, `Iframe`, `age_of_domain`, `DNSRecord`, `web_traffic`, `Page_Rank`, `Google_Index`, `Links_pointing_to_page`, `Statistical_report`

## Setup

```bash
git clone https://github.com/mmiefm1/NetworkSecurity
cd NetworkSecurity
pip install -r requirements.txt
```

Add your credentials to a `.env` file:
```
MONGODB_URL_KEY=your_mongodb_connection_string
```

Run the FastAPI server:
```bash
python app.py
```

## API Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/train` | GET | Triggers the full training pipeline |
| `/predict` | POST | Upload a CSV and get phishing predictions |
| `/docs` | GET | Interactive Swagger UI |

<!-- ## Docker

```bash
docker build -t network-security .
docker run -p 8000:8000 network-security
``` -->

## Experiment Tracking

All training runs are logged to DagsHub via MLflow, including:
- F1 Score
- Precision
- Recall

View experiments at: [dagshub.com/mmiefm1/NetworkSecurity](https://dagshub.com/mmiefm1/NetworkSecurity)
