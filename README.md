# Predictive Decision Support System (PDSS)

## AI-Powered Vendor Risk & Supply Chain Continuity Platform

PDSS (Predictive Decision Support System) is a full-stack application for **vendor management, supply-chain risk analysis, delay prediction, cost forecasting, and vendor recommendation**.

The system combines a **FastAPI backend**, **React + Vite frontend**, **SQLite database**, and machine-learning models to transform supply-chain data into actionable decision-support insights.

The application provides an interactive dashboard for analyzing vendor performance, identifying risk levels, predicting delays, reviewing forecasts, comparing vendors, and selecting a recommended vendor based on composite risk and reliability metrics.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Application Modules](#application-modules)
- [Machine Learning Pipeline](#machine-learning-pipeline)
- [Risk Scoring](#risk-scoring)
- [Backend Architecture](#backend-architecture)
- [Frontend Architecture](#frontend-architecture)
- [API Endpoints](#api-endpoints)
- [Database and Model Artifacts](#database-and-model-artifacts)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Environment Configuration](#environment-configuration)
- [Application Verification](#application-verification)
- [Troubleshooting](#troubleshooting)
- [Git and Security Guidelines](#git-and-security-guidelines)
- [Production Considerations](#production-considerations)
- [Future Enhancements](#future-enhancements)
- [Technical Skills Demonstrated](#technical-skills-demonstrated)
- [Project Status](#project-status)

---

## Project Overview

Modern supply chains are influenced by delivery delays, vendor reliability, quality issues, cost fluctuations, demand variability, and external operational risks.

PDSS is designed to bring these factors together in a single decision-support workflow.

The application uses historical supply-chain data and trained machine-learning models to provide:

- Vendor-level risk analysis
- Delivery delay prediction
- Supply-chain risk classification
- Forecasted vendor cost trends
- Vendor comparison
- Vendor recommendation
- Risk visualization
- Dashboard-level KPIs
- Scenario-based prediction

The objective is to help users move from **reactive vendor monitoring toward predictive and data-driven supply-chain decision-making**.

---

## Objectives

The main objectives of PDSS are to:

1. Predict potential vendor delivery delays.
2. Classify vendors according to supply-chain risk.
3. Calculate a composite risk score from multiple risk factors.
4. Forecast future vendor-related cost trends.
5. Compare vendor performance and reliability.
6. Recommend a suitable vendor using risk and reliability metrics.
7. Provide an interactive dashboard for decision support.
8. Store prediction and analytical results for application use.

---

## Key Features

### 1. Vendor Management

The system retrieves vendor information from the backend database and presents it through the dashboard.

Users can review vendor information and use it as the basis for comparison, forecasting, and risk analysis.

### 2. Delay Prediction

The application provides a prediction endpoint for determining whether a supply-chain delay is likely.

The delay prediction response includes:

- Prediction label
- Prediction probability
- Composite risk score
- Risk level
- Prediction timestamp

### 3. Risk Prediction

PDSS provides a separate risk-prediction workflow that evaluates vendor/supply-chain input data and returns:

- Risk classification
- Model confidence
- Composite risk score
- Risk level
- Prediction timestamp

### 4. Risk Heatmap

The frontend provides a Risk Heatmap view for visually reviewing vendor risk information.

### 5. Vendor Comparison

The Vendor Comparison page provides a vendor-ranking view to support comparison of available vendors.

### 6. Forecasting

The Forecast page retrieves forecast information for a selected vendor.

Forecast results contain:

- Forecast month
- Predicted cost
- Lower bound
- Upper bound

### 7. Vendor Recommendation

PDSS includes an automated vendor recommendation endpoint.

The recommendation considers vendors with available risk and metric records and selects the candidate with the **lowest composite risk score**, using the **strongest reliability index as the tie-breaker**.

### 8. Dashboard Analytics

The dashboard summary provides application-level metrics including:

- Total vendors
- Average risk score
- High-risk vendor count
- Delay predictions generated today
- Risk predictions generated today
- Forecast point count
- Model metrics

### 9. Scenario Simulator

The Scenario Simulator allows users to submit scenario inputs and run the delay and risk prediction workflows through the backend API.

---

# System Architecture

```text
                         ┌──────────────────────┐
                         │        User          │
                         └──────────┬───────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────┐
                    │       React + Vite           │
                    │        Frontend              │
                    │                              │
                    │  Dashboard                   │
                    │  Vendor Comparison           │
                    │  Risk Heatmap                │
                    │  Forecast                    │
                    │  Scenario Simulator           │
                    └──────────────┬───────────────┘
                                   │
                              Axios / HTTP
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │         FastAPI              │
                    │          Backend             │
                    │                              │
                    │ Vendor APIs                  │
                    │ Prediction APIs              │
                    │ Forecast API                 │
                    │ Recommendation API           │
                    │ Dashboard API                │
                    └──────────────┬───────────────┘
                                   │
                  ┌────────────────┼─────────────────┐
                  │                │                 │
                  ▼                ▼                 ▼
        ┌────────────────┐ ┌───────────────┐ ┌─────────────────┐
        │ ML Model       │ │ SQLite        │ │ Forecast Data   │
        │ Artifacts      │ │ Database      │ │ & Metrics       │
        │                │ │               │ │                 │
        │ XGBoost        │ │ Vendors       │ │ Forecasts       │
        │ LightGBM       │ │ Predictions   │ │ Model metrics   │
        │ Prophet        │ │ Risk scores   │ │                 │
        └────────────────┘ └───────────────┘ └─────────────────┘
```

---

# Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 |
| Frontend Build Tool | Vite 6 |
| UI Styling | Tailwind CSS |
| Charts | Recharts |
| HTTP Client | Axios |
| Backend | FastAPI |
| API Server | Uvicorn |
| Language | Python |
| Database | SQLite |
| ORM / Database Access | SQLAlchemy |
| Data Processing | Pandas, NumPy |
| ML | XGBoost, LightGBM, Scikit-learn |
| Forecasting | Prophet |
| Hyperparameter Optimization | Optuna |
| Explainability | SHAP |
| API Validation | Pydantic |
| API Documentation | FastAPI / OpenAPI / Swagger |
| Version Control | Git |

---

# Project Structure

```text
Predictive-Decision-Support-System-main/
│
├── .gitignore
│
├── README.md
│
└── PDSS/
    │
    ├── backend/
    │   ├── app/
    │   │   ├── __init__.py
    │   │   ├── database.py
    │   │   ├── main.py
    │   │   ├── ml_pipeline.py
    │   │   ├── models_loader.py
    │   │   ├── risk_engine.py
    │   │   │
    │   │   ├── routes/
    │   │   │   ├── __init__.py
    │   │   │   └── api.py
    │   │   │
    │   │   └── schemas/
    │   │       ├── __init__.py
    │   │       └── api.py
    │   │
    │   ├── data/
    │   │   └── raw/
    │   │
    │   ├── models/
    │   │   ├── delay_model.pkl
    │   │   ├── risk_model.pkl
    │   │   ├── prophet_model.pkl
    │   │   ├── forecast_template.json
    │   │   ├── metrics.json
    │   │   ├── feature_importance.png
    │   │   └── shap_summary.png
    │   │
    │   ├── supply_chain.db
    │   ├── requirements.txt
    │   └── package-lock.json
    │
    ├── frontend/
    │   ├── src/
    │   │   ├── App.jsx
    │   │   ├── main.jsx
    │   │   ├── index.css
    │   │   ├── charts/
    │   │   ├── components/
    │   │   ├── pages/
    │   │   └── services/
    │   │
    │   ├── index.html
    │   ├── package.json
    │   ├── vite.config.js
    │   ├── tailwind.config.js
    │   └── postcss.config.js
    │
    ├── kaggle.json
    └── package-lock.json
```

> The repository also contains a small React/Vite demo application under `frontend/demo/demo/`. Its README is the standard Vite template documentation and is separate from the main PDSS application documentation.

---

# Application Modules

## Dashboard

The Dashboard brings together the primary operational and model metrics.

It uses:

- Dashboard summary
- Vendor information
- Vendor recommendation
- Forecast information

## Vendor Comparison

Provides a ranking-oriented view of available vendors to support comparative analysis.

## Risk Heatmap

Provides a visual representation of vendor risk information.

## Forecast

Allows users to select a vendor and retrieve its available forecast points.

## Scenario Simulator

Provides a user interface for submitting prediction inputs and evaluating delay/risk predictions.

---

# Machine Learning Pipeline

The backend contains a dedicated machine-learning pipeline in:

```text
backend/app/ml_pipeline.py
```

The pipeline is organized into multiple stages.

### Pipeline Flow

```text
Kaggle Dataset Access
        │
        ▼
Dataset Download & Validation
        │
        ▼
Data Loading & Preprocessing
        │
        ▼
Feature Preparation
        │
        ├───────────────┐
        ▼               ▼
Delay Model       Risk Model
XGBoost           LightGBM
        │               │
        └───────┬───────┘
                ▼
        Cost Forecasting
             Prophet
                │
                ▼
       SHAP Explainability
                │
                ▼
       Model Artifacts
                │
                ▼
       SQLite Population
```

### Models Used

#### XGBoost

Used for the delay-prediction workflow, with the pipeline containing Optuna-based tuning.

#### LightGBM

Used for the supply-chain risk classification workflow.

#### Prophet

Used for cost forecasting.

#### SHAP

Used to generate model explainability artifacts such as feature-importance information and SHAP summaries.

### Model Artifacts

The repository contains:

```text
backend/models/
├── delay_model.pkl
├── risk_model.pkl
├── prophet_model.pkl
├── forecast_template.json
├── metrics.json
├── feature_importance.png
└── shap_summary.png
```

---

# Risk Scoring

In addition to model predictions, the backend calculates a composite risk score using five normalized inputs:

| Risk Factor | Weight |
|---|---:|
| Delay Probability | 30% |
| Quality Risk | 25% |
| Cost Volatility | 20% |
| External Risk Proxy | 15% |
| Demand Variability | 10% |

The resulting score is converted into a 0–100 scale.

### Risk Classification

| Score | Risk Level |
|---:|---|
| 0–39.99 | Low |
| 40–69.99 | Medium |
| 70–100 | High |

This scoring logic is implemented in:

```text
backend/app/risk_engine.py
```

---

# Backend Architecture

The backend is built with FastAPI.

Important application modules include:

```text
backend/app/
├── main.py
├── database.py
├── ml_pipeline.py
├── models_loader.py
├── risk_engine.py
├── routes/
└── schemas/
```

### Responsibilities

**`main.py`**

- Creates the FastAPI application
- Configures CORS
- Creates database tables during startup
- Runs the ML pipeline during startup
- Loads model artifacts
- Starts Uvicorn

**`database.py`**

Handles SQLAlchemy database models and database access.

**`ml_pipeline.py`**

Contains dataset processing, model training, forecasting, explainability, artifact persistence, and database population logic.

**`models_loader.py`**

Loads trained model artifacts and associated metrics for API inference.

**`risk_engine.py`**

Calculates composite risk scores and risk levels.

**`routes/api.py`**

Contains the application's REST API endpoints.

---

# Frontend Architecture

The frontend is implemented with React and Vite.

```text
frontend/src/
├── App.jsx
├── main.jsx
├── index.css
│
├── charts/
│   ├── ForecastLineChart.jsx
│   ├── RiskHeatmapTable.jsx
│   ├── RiskPieChart.jsx
│   └── VendorBarChart.jsx
│
├── components/
│   ├── KpiCard.jsx
│   ├── RiskAlerts.jsx
│   └── Sidebar.jsx
│
├── pages/
│   ├── Dashboard.jsx
│   ├── ForecastPage.jsx
│   ├── RiskHeatmap.jsx
│   ├── ScenarioSimulator.jsx
│   └── VendorComparison.jsx
│
└── services/
    └── api.js
```

The frontend uses **Axios** to communicate with the FastAPI backend.

The configured API client currently uses:

```text
http://localhost:8000
```

as its backend base URL.

---

# API Endpoints

The backend exposes the following application endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/vendors` | Retrieve vendor information |
| GET | `/dashboard-summary` | Retrieve dashboard KPIs and model metrics |
| GET | `/forecast/{vendor_id}` | Retrieve forecast data for a vendor |
| GET | `/recommend-best-vendor` | Recommend the best available vendor |
| POST | `/predict-delay` | Predict supply-chain delay |
| POST | `/predict-risk` | Predict vendor/supply-chain risk |

### Swagger Documentation

When the backend is running:

```text
http://localhost:8000/docs
```

FastAPI automatically exposes interactive Swagger/OpenAPI documentation.

---

# Database and Model Artifacts

The application uses:

```text
backend/supply_chain.db
```

The database stores application data related to vendors, metrics, forecasts, predictions, and risk scores.

The backend also maintains trained model artifacts under:

```text
backend/models/
```

This allows the application to load trained models and use them for inference.

---

# Prerequisites

Before running PDSS, install:

- Python 3.10 or newer
- Node.js 18 or newer
- npm

For the machine-learning pipeline, the repository's backend dependencies include:

- XGBoost
- LightGBM
- Prophet
- SHAP
- Optuna
- Scikit-learn
- Pandas
- NumPy
- Kaggle CLI

---

# Installation and Setup

## 1. Open the PDSS directory

From the repository root:

```powershell
cd Predictive-Decision-Support-System-main\PDSS
```

---

## 2. Create the Python virtual environment

### Windows PowerShell

```powershell
cd backend
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
cd ..
```

### macOS/Linux

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cd ..
```

---

## 3. Configure Kaggle credentials

The current backend startup implementation invokes the full ML pipeline.

The pipeline verifies Kaggle API access and may download/validate datasets before training and persisting model artifacts.

Kaggle credentials should therefore be configured before starting the backend.

The expected standard location is:

```text
~/.kaggle/kaggle.json
```

The project also contains a root-level `kaggle.json` that the pipeline can copy into the user's `.kaggle` directory when appropriate.

**Do not commit real Kaggle credentials to GitHub.**

If credentials have been exposed publicly, revoke or rotate them through Kaggle.

---

## 4. Start the backend

From the `PDSS` directory:

```powershell
python backend/app/main.py
```

The FastAPI server runs on:

```text
http://localhost:8000
```

Swagger documentation:

```text
http://localhost:8000/docs
```

### Important Startup Behavior

The current implementation runs:

```text
Database initialization
        ↓
Full ML pipeline
        ↓
Model loading
        ↓
API serving
```

Therefore, the first startup can take significantly longer than a simple API-only application because dataset access, preprocessing, model training, forecasting, explainability, and artifact persistence are part of the startup workflow.

---

# Start the Frontend

Open a second terminal.

```powershell
cd Predictive-Decision-Support-System-main\PDSS\frontend
npm install
npm run dev
```

Vite normally starts the frontend at:

```text
http://localhost:5173
```

Open that URL in a browser after the backend has started successfully.

---

# Application Verification

Use the following sequence to verify the project:

### Step 1 — Backend

Open:

```text
http://localhost:8000/docs
```

Confirm that the Swagger interface loads.

### Step 2 — Frontend

Open:

```text
http://localhost:5173
```

Confirm that the dashboard loads.

### Step 3 — Dashboard

Verify:

- Vendor information
- KPI cards
- Risk information
- Model metrics
- Vendor recommendation

### Step 4 — Analytics Pages

Test:

- Vendor Comparison
- Risk Heatmap
- Forecast

### Step 5 — Scenario Simulator

Submit scenario inputs and verify that delay and risk prediction responses are returned by the backend.

---

# Troubleshooting

## `ModuleNotFoundError`

Make sure the backend virtual environment is active:

```powershell
cd backend
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

---

## `vite is not recognized`

From the frontend directory:

```powershell
npm install
npm run dev
```

---

## Backend does not start because Kaggle access fails

The current startup pipeline verifies Kaggle API access.

Check:

```text
~/.kaggle/kaggle.json
```

and verify that the Kaggle CLI is installed through the backend requirements:

```powershell
pip install -r backend/requirements.txt
```

---

## Backend cannot load model artifacts

Check:

```text
backend/models/
```

for:

```text
delay_model.pkl
risk_model.pkl
prophet_model.pkl
forecast_template.json
metrics.json
```

If these artifacts need to be regenerated, the ML pipeline must complete successfully.

---

## Frontend cannot connect to the backend

Confirm that FastAPI is running:

```text
http://localhost:8000
```

Also verify the frontend API configuration in:

```text
frontend/src/services/api.js
```

The current API client is configured with:

```javascript
baseURL: 'http://localhost:8000'
```

---

# Git and Security Guidelines

Do not commit sensitive or generated development files such as:

```text
kaggle.json
.env
.env.*
.venv/
node_modules/
```

Before pushing changes:

```bash
git status
git add .
git commit -m "Update PDSS documentation"
git push
```

Always inspect `git status` before committing to ensure that credentials and unnecessary generated files are not included.

---

# Production Considerations

The current repository is structured as a development/portfolio application.

For production deployment, the following areas should be strengthened:

- Use a managed database such as PostgreSQL
- Move secrets to a secure secret-management system
- Remove credentials from the repository
- Restrict CORS origins
- Separate model training from API startup
- Use scheduled or pipeline-based model retraining
- Add authentication and authorization
- Add structured application logging
- Add monitoring and model-performance tracking
- Containerize services with Docker
- Add CI/CD automation
- Use production-grade API deployment

In particular, running the complete ML training pipeline during API startup is suitable for the current project workflow but would normally be separated from the production API service.

---

# Future Enhancements

Potential extensions include:

- Role-based user authentication
- Vendor-specific alert notifications
- Automated risk alerts
- Real-time supply-chain data integration
- Scheduled model retraining
- Model drift monitoring
- Advanced vendor recommendation strategies
- Cloud deployment
- PostgreSQL integration
- Docker-based deployment
- CI/CD pipeline
- Advanced analytics and reporting
- Additional explainability dashboards

---

# Technical Skills Demonstrated

This project demonstrates practical experience with:

### Programming

- Python
- JavaScript

### Backend

- FastAPI
- REST APIs
- Uvicorn
- SQLAlchemy
- Pydantic

### Frontend

- React
- Vite
- React Router
- Tailwind CSS
- Axios
- Recharts

### Machine Learning

- XGBoost
- LightGBM
- Scikit-learn
- Prophet
- Optuna
- SHAP

### Data & Analytics

- Pandas
- NumPy
- SQLite
- Supply-chain analytics
- Risk scoring
- Forecasting
- Predictive modeling

### Development Tools

- Git
- GitHub
- Swagger / OpenAPI
- npm
- Python virtual environments

---

# Project Status

**Status:** Functional Full-Stack AI/ML Project

PDSS combines predictive modeling, risk scoring, forecasting, vendor analytics, and a React-based visualization layer into a single supply-chain decision-support application.

The project demonstrates the integration of **machine learning + backend APIs + database persistence + interactive frontend analytics** within a full-stack application.

---

## Author

**Talapaneni Balaji**

B.Tech – Artificial Intelligence & Data Science  
2026 Graduate

---

## License

This project is intended for educational, demonstration, and portfolio purposes unless a separate license is provided with the repository.
# Predictive Decision Support System (PDSS)

## AI-Powered Vendor Risk & Supply Chain Continuity Platform

PDSS (Predictive Decision Support System) is a full-stack application for **vendor management, supply-chain risk analysis, delay prediction, cost forecasting, and vendor recommendation**.

The system combines a **FastAPI backend**, **React + Vite frontend**, **SQLite database**, and machine-learning models to transform supply-chain data into actionable decision-support insights.

The application provides an interactive dashboard for analyzing vendor performance, identifying risk levels, predicting delays, reviewing forecasts, comparing vendors, and selecting a recommended vendor based on composite risk and reliability metrics.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Application Modules](#application-modules)
- [Machine Learning Pipeline](#machine-learning-pipeline)
- [Risk Scoring](#risk-scoring)
- [Backend Architecture](#backend-architecture)
- [Frontend Architecture](#frontend-architecture)
- [API Endpoints](#api-endpoints)
- [Database and Model Artifacts](#database-and-model-artifacts)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Environment Configuration](#environment-configuration)
- [Application Verification](#application-verification)
- [Troubleshooting](#troubleshooting)
- [Git and Security Guidelines](#git-and-security-guidelines)
- [Production Considerations](#production-considerations)
- [Future Enhancements](#future-enhancements)
- [Technical Skills Demonstrated](#technical-skills-demonstrated)
- [Project Status](#project-status)

---

## Project Overview

Modern supply chains are influenced by delivery delays, vendor reliability, quality issues, cost fluctuations, demand variability, and external operational risks.

PDSS is designed to bring these factors together in a single decision-support workflow.

The application uses historical supply-chain data and trained machine-learning models to provide:

- Vendor-level risk analysis
- Delivery delay prediction
- Supply-chain risk classification
- Forecasted vendor cost trends
- Vendor comparison
- Vendor recommendation
- Risk visualization
- Dashboard-level KPIs
- Scenario-based prediction

The objective is to help users move from **reactive vendor monitoring toward predictive and data-driven supply-chain decision-making**.

---

## Objectives

The main objectives of PDSS are to:

1. Predict potential vendor delivery delays.
2. Classify vendors according to supply-chain risk.
3. Calculate a composite risk score from multiple risk factors.
4. Forecast future vendor-related cost trends.
5. Compare vendor performance and reliability.
6. Recommend a suitable vendor using risk and reliability metrics.
7. Provide an interactive dashboard for decision support.
8. Store prediction and analytical results for application use.

---

## Key Features

### 1. Vendor Management

The system retrieves vendor information from the backend database and presents it through the dashboard.

Users can review vendor information and use it as the basis for comparison, forecasting, and risk analysis.

### 2. Delay Prediction

The application provides a prediction endpoint for determining whether a supply-chain delay is likely.

The delay prediction response includes:

- Prediction label
- Prediction probability
- Composite risk score
- Risk level
- Prediction timestamp

### 3. Risk Prediction

PDSS provides a separate risk-prediction workflow that evaluates vendor/supply-chain input data and returns:

- Risk classification
- Model confidence
- Composite risk score
- Risk level
- Prediction timestamp

### 4. Risk Heatmap

The frontend provides a Risk Heatmap view for visually reviewing vendor risk information.

### 5. Vendor Comparison

The Vendor Comparison page provides a vendor-ranking view to support comparison of available vendors.

### 6. Forecasting

The Forecast page retrieves forecast information for a selected vendor.

Forecast results contain:

- Forecast month
- Predicted cost
- Lower bound
- Upper bound

### 7. Vendor Recommendation

PDSS includes an automated vendor recommendation endpoint.

The recommendation considers vendors with available risk and metric records and selects the candidate with the **lowest composite risk score**, using the **strongest reliability index as the tie-breaker**.

### 8. Dashboard Analytics

The dashboard summary provides application-level metrics including:

- Total vendors
- Average risk score
- High-risk vendor count
- Delay predictions generated today
- Risk predictions generated today
- Forecast point count
- Model metrics

### 9. Scenario Simulator

The Scenario Simulator allows users to submit scenario inputs and run the delay and risk prediction workflows through the backend API.

---

# System Architecture

```text
                         ┌──────────────────────┐
                         │        User          │
                         └──────────┬───────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────┐
                    │       React + Vite           │
                    │        Frontend              │
                    │                              │
                    │  Dashboard                   │
                    │  Vendor Comparison           │
                    │  Risk Heatmap                │
                    │  Forecast                    │
                    │  Scenario Simulator           │
                    └──────────────┬───────────────┘
                                   │
                              Axios / HTTP
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │         FastAPI              │
                    │          Backend             │
                    │                              │
                    │ Vendor APIs                  │
                    │ Prediction APIs              │
                    │ Forecast API                 │
                    │ Recommendation API           │
                    │ Dashboard API                │
                    └──────────────┬───────────────┘
                                   │
                  ┌────────────────┼─────────────────┐
                  │                │                 │
                  ▼                ▼                 ▼
        ┌────────────────┐ ┌───────────────┐ ┌─────────────────┐
        │ ML Model       │ │ SQLite        │ │ Forecast Data   │
        │ Artifacts      │ │ Database      │ │ & Metrics       │
        │                │ │               │ │                 │
        │ XGBoost        │ │ Vendors       │ │ Forecasts       │
        │ LightGBM       │ │ Predictions   │ │ Model metrics   │
        │ Prophet        │ │ Risk scores   │ │                 │
        └────────────────┘ └───────────────┘ └─────────────────┘
```

---

# Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 |
| Frontend Build Tool | Vite 6 |
| UI Styling | Tailwind CSS |
| Charts | Recharts |
| HTTP Client | Axios |
| Backend | FastAPI |
| API Server | Uvicorn |
| Language | Python |
| Database | SQLite |
| ORM / Database Access | SQLAlchemy |
| Data Processing | Pandas, NumPy |
| ML | XGBoost, LightGBM, Scikit-learn |
| Forecasting | Prophet |
| Hyperparameter Optimization | Optuna |
| Explainability | SHAP |
| API Validation | Pydantic |
| API Documentation | FastAPI / OpenAPI / Swagger |
| Version Control | Git |

---

# Project Structure

```text
Predictive-Decision-Support-System-main/
│
├── .gitignore
│
├── README.md
│
└── PDSS/
    │
    ├── backend/
    │   ├── app/
    │   │   ├── __init__.py
    │   │   ├── database.py
    │   │   ├── main.py
    │   │   ├── ml_pipeline.py
    │   │   ├── models_loader.py
    │   │   ├── risk_engine.py
    │   │   │
    │   │   ├── routes/
    │   │   │   ├── __init__.py
    │   │   │   └── api.py
    │   │   │
    │   │   └── schemas/
    │   │       ├── __init__.py
    │   │       └── api.py
    │   │
    │   ├── data/
    │   │   └── raw/
    │   │
    │   ├── models/
    │   │   ├── delay_model.pkl
    │   │   ├── risk_model.pkl
    │   │   ├── prophet_model.pkl
    │   │   ├── forecast_template.json
    │   │   ├── metrics.json
    │   │   ├── feature_importance.png
    │   │   └── shap_summary.png
    │   │
    │   ├── supply_chain.db
    │   ├── requirements.txt
    │   └── package-lock.json
    │
    ├── frontend/
    │   ├── src/
    │   │   ├── App.jsx
    │   │   ├── main.jsx
    │   │   ├── index.css
    │   │   ├── charts/
    │   │   ├── components/
    │   │   ├── pages/
    │   │   └── services/
    │   │
    │   ├── index.html
    │   ├── package.json
    │   ├── vite.config.js
    │   ├── tailwind.config.js
    │   └── postcss.config.js
    │
    ├── kaggle.json
    └── package-lock.json
```

> The repository also contains a small React/Vite demo application under `frontend/demo/demo/`. Its README is the standard Vite template documentation and is separate from the main PDSS application documentation.

---

# Application Modules

## Dashboard

The Dashboard brings together the primary operational and model metrics.

It uses:

- Dashboard summary
- Vendor information
- Vendor recommendation
- Forecast information

## Vendor Comparison

Provides a ranking-oriented view of available vendors to support comparative analysis.

## Risk Heatmap

Provides a visual representation of vendor risk information.

## Forecast

Allows users to select a vendor and retrieve its available forecast points.

## Scenario Simulator

Provides a user interface for submitting prediction inputs and evaluating delay/risk predictions.

---

# Machine Learning Pipeline

The backend contains a dedicated machine-learning pipeline in:

```text
backend/app/ml_pipeline.py
```

The pipeline is organized into multiple stages.

### Pipeline Flow

```text
Kaggle Dataset Access
        │
        ▼
Dataset Download & Validation
        │
        ▼
Data Loading & Preprocessing
        │
        ▼
Feature Preparation
        │
        ├───────────────┐
        ▼               ▼
Delay Model       Risk Model
XGBoost           LightGBM
        │               │
        └───────┬───────┘
                ▼
        Cost Forecasting
             Prophet
                │
                ▼
       SHAP Explainability
                │
                ▼
       Model Artifacts
                │
                ▼
       SQLite Population
```

### Models Used

#### XGBoost

Used for the delay-prediction workflow, with the pipeline containing Optuna-based tuning.

#### LightGBM

Used for the supply-chain risk classification workflow.

#### Prophet

Used for cost forecasting.

#### SHAP

Used to generate model explainability artifacts such as feature-importance information and SHAP summaries.

### Model Artifacts

The repository contains:

```text
backend/models/
├── delay_model.pkl
├── risk_model.pkl
├── prophet_model.pkl
├── forecast_template.json
├── metrics.json
├── feature_importance.png
└── shap_summary.png
```

---

# Risk Scoring

In addition to model predictions, the backend calculates a composite risk score using five normalized inputs:

| Risk Factor | Weight |
|---|---:|
| Delay Probability | 30% |
| Quality Risk | 25% |
| Cost Volatility | 20% |
| External Risk Proxy | 15% |
| Demand Variability | 10% |

The resulting score is converted into a 0–100 scale.

### Risk Classification

| Score | Risk Level |
|---:|---|
| 0–39.99 | Low |
| 40–69.99 | Medium |
| 70–100 | High |

This scoring logic is implemented in:

```text
backend/app/risk_engine.py
```

---

# Backend Architecture

The backend is built with FastAPI.

Important application modules include:

```text
backend/app/
├── main.py
├── database.py
├── ml_pipeline.py
├── models_loader.py
├── risk_engine.py
├── routes/
└── schemas/
```

### Responsibilities

**`main.py`**

- Creates the FastAPI application
- Configures CORS
- Creates database tables during startup
- Runs the ML pipeline during startup
- Loads model artifacts
- Starts Uvicorn

**`database.py`**

Handles SQLAlchemy database models and database access.

**`ml_pipeline.py`**

Contains dataset processing, model training, forecasting, explainability, artifact persistence, and database population logic.

**`models_loader.py`**

Loads trained model artifacts and associated metrics for API inference.

**`risk_engine.py`**

Calculates composite risk scores and risk levels.

**`routes/api.py`**

Contains the application's REST API endpoints.

---

# Frontend Architecture

The frontend is implemented with React and Vite.

```text
frontend/src/
├── App.jsx
├── main.jsx
├── index.css
│
├── charts/
│   ├── ForecastLineChart.jsx
│   ├── RiskHeatmapTable.jsx
│   ├── RiskPieChart.jsx
│   └── VendorBarChart.jsx
│
├── components/
│   ├── KpiCard.jsx
│   ├── RiskAlerts.jsx
│   └── Sidebar.jsx
│
├── pages/
│   ├── Dashboard.jsx
│   ├── ForecastPage.jsx
│   ├── RiskHeatmap.jsx
│   ├── ScenarioSimulator.jsx
│   └── VendorComparison.jsx
│
└── services/
    └── api.js
```

The frontend uses **Axios** to communicate with the FastAPI backend.

The configured API client currently uses:

```text
http://localhost:8000
```

as its backend base URL.

---

# API Endpoints

The backend exposes the following application endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/vendors` | Retrieve vendor information |
| GET | `/dashboard-summary` | Retrieve dashboard KPIs and model metrics |
| GET | `/forecast/{vendor_id}` | Retrieve forecast data for a vendor |
| GET | `/recommend-best-vendor` | Recommend the best available vendor |
| POST | `/predict-delay` | Predict supply-chain delay |
| POST | `/predict-risk` | Predict vendor/supply-chain risk |

### Swagger Documentation

When the backend is running:

```text
http://localhost:8000/docs
```

FastAPI automatically exposes interactive Swagger/OpenAPI documentation.

---

# Database and Model Artifacts

The application uses:

```text
backend/supply_chain.db
```

The database stores application data related to vendors, metrics, forecasts, predictions, and risk scores.

The backend also maintains trained model artifacts under:

```text
backend/models/
```

This allows the application to load trained models and use them for inference.

---

# Prerequisites

Before running PDSS, install:

- Python 3.10 or newer
- Node.js 18 or newer
- npm

For the machine-learning pipeline, the repository's backend dependencies include:

- XGBoost
- LightGBM
- Prophet
- SHAP
- Optuna
- Scikit-learn
- Pandas
- NumPy
- Kaggle CLI

---

# Installation and Setup

## 1. Open the PDSS directory

From the repository root:

```powershell
cd Predictive-Decision-Support-System-main\PDSS
```

---

## 2. Create the Python virtual environment

### Windows PowerShell

```powershell
cd backend
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
cd ..
```

### macOS/Linux

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cd ..
```

---

## 3. Configure Kaggle credentials

The current backend startup implementation invokes the full ML pipeline.

The pipeline verifies Kaggle API access and may download/validate datasets before training and persisting model artifacts.

Kaggle credentials should therefore be configured before starting the backend.

The expected standard location is:

```text
~/.kaggle/kaggle.json
```

The project also contains a root-level `kaggle.json` that the pipeline can copy into the user's `.kaggle` directory when appropriate.

**Do not commit real Kaggle credentials to GitHub.**

If credentials have been exposed publicly, revoke or rotate them through Kaggle.

---

## 4. Start the backend

From the `PDSS` directory:

```powershell
python backend/app/main.py
```

The FastAPI server runs on:

```text
http://localhost:8000
```

Swagger documentation:

```text
http://localhost:8000/docs
```

### Important Startup Behavior

The current implementation runs:

```text
Database initialization
        ↓
Full ML pipeline
        ↓
Model loading
        ↓
API serving
```

Therefore, the first startup can take significantly longer than a simple API-only application because dataset access, preprocessing, model training, forecasting, explainability, and artifact persistence are part of the startup workflow.

---

# Start the Frontend

Open a second terminal.

```powershell
cd Predictive-Decision-Support-System-main\PDSS\frontend
npm install
npm run dev
```

Vite normally starts the frontend at:

```text
http://localhost:5173
```

Open that URL in a browser after the backend has started successfully.

---

# Application Verification

Use the following sequence to verify the project:

### Step 1 — Backend

Open:

```text
http://localhost:8000/docs
```

Confirm that the Swagger interface loads.

### Step 2 — Frontend

Open:

```text
http://localhost:5173
```

Confirm that the dashboard loads.

### Step 3 — Dashboard

Verify:

- Vendor information
- KPI cards
- Risk information
- Model metrics
- Vendor recommendation

### Step 4 — Analytics Pages

Test:

- Vendor Comparison
- Risk Heatmap
- Forecast

### Step 5 — Scenario Simulator

Submit scenario inputs and verify that delay and risk prediction responses are returned by the backend.

---

# Troubleshooting

## `ModuleNotFoundError`

Make sure the backend virtual environment is active:

```powershell
cd backend
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

---

## `vite is not recognized`

From the frontend directory:

```powershell
npm install
npm run dev
```

---

## Backend does not start because Kaggle access fails

The current startup pipeline verifies Kaggle API access.

Check:

```text
~/.kaggle/kaggle.json
```

and verify that the Kaggle CLI is installed through the backend requirements:

```powershell
pip install -r backend/requirements.txt
```

---

## Backend cannot load model artifacts

Check:

```text
backend/models/
```

for:

```text
delay_model.pkl
risk_model.pkl
prophet_model.pkl
forecast_template.json
metrics.json
```

If these artifacts need to be regenerated, the ML pipeline must complete successfully.

---

## Frontend cannot connect to the backend

Confirm that FastAPI is running:

```text
http://localhost:8000
```

Also verify the frontend API configuration in:

```text
frontend/src/services/api.js
```

The current API client is configured with:

```javascript
baseURL: 'http://localhost:8000'
```

---

# Git and Security Guidelines

Do not commit sensitive or generated development files such as:

```text
kaggle.json
.env
.env.*
.venv/
node_modules/
```

Before pushing changes:

```bash
git status
git add .
git commit -m "Update PDSS documentation"
git push
```

Always inspect `git status` before committing to ensure that credentials and unnecessary generated files are not included.

---

# Production Considerations

The current repository is structured as a development/portfolio application.

For production deployment, the following areas should be strengthened:

- Use a managed database such as PostgreSQL
- Move secrets to a secure secret-management system
- Remove credentials from the repository
- Restrict CORS origins
- Separate model training from API startup
- Use scheduled or pipeline-based model retraining
- Add authentication and authorization
- Add structured application logging
- Add monitoring and model-performance tracking
- Containerize services with Docker
- Add CI/CD automation
- Use production-grade API deployment

In particular, running the complete ML training pipeline during API startup is suitable for the current project workflow but would normally be separated from the production API service.

---

# Future Enhancements

Potential extensions include:

- Role-based user authentication
- Vendor-specific alert notifications
- Automated risk alerts
- Real-time supply-chain data integration
- Scheduled model retraining
- Model drift monitoring
- Advanced vendor recommendation strategies
- Cloud deployment
- PostgreSQL integration
- Docker-based deployment
- CI/CD pipeline
- Advanced analytics and reporting
- Additional explainability dashboards

---

# Technical Skills Demonstrated

This project demonstrates practical experience with:

### Programming

- Python
- JavaScript

### Backend

- FastAPI
- REST APIs
- Uvicorn
- SQLAlchemy
- Pydantic

### Frontend

- React
- Vite
- React Router
- Tailwind CSS
- Axios
- Recharts

### Machine Learning

- XGBoost
- LightGBM
- Scikit-learn
- Prophet
- Optuna
- SHAP

### Data & Analytics

- Pandas
- NumPy
- SQLite
- Supply-chain analytics
- Risk scoring
- Forecasting
- Predictive modeling

### Development Tools

- Git
- GitHub
- Swagger / OpenAPI
- npm
- Python virtual environments

---

# Project Status

**Status:** Functional Full-Stack AI/ML Project

PDSS combines predictive modeling, risk scoring, forecasting, vendor analytics, and a React-based visualization layer into a single supply-chain decision-support application.

The project demonstrates the integration of **machine learning + backend APIs + database persistence + interactive frontend analytics** within a full-stack application.

---

## Author

**Talapaneni Balaji**

B.Tech – Artificial Intelligence & Data Science  
2026 Graduate

---

## License

This project is intended for educational, demonstration, and portfolio purposes unless a separate license is provided with the repository.
