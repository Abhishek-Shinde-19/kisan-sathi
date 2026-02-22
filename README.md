# KISAN - Crop Recommendation & Decision Support System

![Version](https://img.shields.io/badge/version-2026.02-green)
![Backend](https://img.shields.io/badge/backend-Flask%202.3.2-black)
![Frontend](https://img.shields.io/badge/frontend-React%2018.2.0-61dafb)
![ML](https://img.shields.io/badge/ML-RandomForest-blue)

KISAN is a full-stack agri-decision platform for farmers. It combines ML-based crop advice, mandi/market intelligence, global export analytics, and multilingual UX in one app.

## What Is Implemented (Current)

- Crop recommendation engine (top 5 crops with confidence)
- Yield prediction endpoint
- Location-assisted soil defaults (`state` + optional `district`)
- Soil testing center suggestions by state
- Market insights with trend/stability + forecast
- Historical mandi chart APIs (`/api/agmarket/history`)
- Live mandi fetch pipeline (`data.gov.in` primary, CEDA fallback)
- Global market access module (countries, commodities, top exporters, trends, demand forecast)
- 12-language UI support using Google Translate integration
- React pages: Home, Recommendation, Results, Market Insights, Global Market Access

## Team Modules In Progress

- WhatsApp chatbot integration (friend’s module) for conversational farmer support
- Government schemes information section (friend’s module) for farmer welfare programs

These are intentionally documented here so the README reflects active branch work even if integration to `main` is still underway.

## Project Structure

```text
kaisan/
├── app.py
├── requirements.txt
├── data/
│   ├── models/
│   ├── processed/
│   └── kaggel/
├── training/
└── frontend/
    ├── package.json
    └── src/
        ├── components/
        ├── pages/
        └── styles/
```

## Tech Stack

- Backend: Flask, Flask-CORS, pandas, scikit-learn, NumPy
- Frontend: React, React Router, Recharts, Lucide icons
- Data/ML: joblib model artifacts + processed CSV datasets
- External market integrations:
  - data.gov.in API
  - CEDA Agmarknet API

## Quick Start

### 1) Backend

```bash
python -m venv venv
venv\Scripts\activate   # Windows
pip install -r requirements.txt
python app.py
```

Backend: `http://localhost:5000`

### 2) Frontend

```bash
cd frontend
npm install
npm start
```

Frontend: `http://localhost:3000`

## Environment Variables (Optional but Recommended)

Create a `.env` in project root:

```env
AGMARKET_API_KEY=your_key_here
DATA_GOV_IN_API_KEY=your_key_here
```

If keys are missing, live mandi endpoints degrade gracefully (no crash), but live records may be unavailable.

## API Overview

### Core Recommendation APIs

- `GET /api/health`
- `GET /api/crops/list`
- `GET /api/locations`
- `GET /api/soil-data?state={state}&district={district}`
- `GET /api/testing-centers?state={state}`
- `POST /api/recommend-crop`
- `POST /api/yield-prediction`
- `GET /api/model-info`
- `GET /api/feature-importance`

### Market & Mandi APIs

- `GET /api/market-insights/{crop}`
- `GET /api/market-insights/{crop}/chart-data`
- `GET /api/agmarket/history?commodity={name}&days=90`
- `GET /api/agmarket/live?commodity={name}&source=api`
- `GET /api/ceda/commodities`
- `GET /api/seasonal-recommendations/{season}`

### Global Market APIs

- `GET /api/global/countries`
- `GET /api/global/commodities`
- `GET /api/global/export-by-country/{country}`
- `GET /api/global/export-demand?commodity={name}`
- `GET /api/global/commodity-trend/{commodity}`
- `GET /api/global/top-exporters?commodity={name}&year=2024&limit=10`
- `GET /api/global/country-commodities/{country}?year=2024&limit=20`
- `GET /api/global/demand-forecast?commodity={name}&country={country}`

## Frontend Routes

- `/` Home
- `/recommend` Recommendation form (manual + location-assisted)
- `/results` Recommendation output
- `/market-insights` Mandi bhav + trends + charts
- `/global-market` International demand/export analysis

## ML Summary

- Crop classifier: RandomForestClassifier
- Yield estimator: RandomForestRegressor
- Feature scaling: StandardScaler
- Input vector includes N, P, K, temperature, humidity, pH, rainfall + engineered features

## Data Sources in Repo

- Raw datasets: `data/kaggel/`
- Processed datasets: `data/processed/`
- Model artifacts: `data/models/`

Primary files used at runtime include:

- `data/processed/merged_training_data.csv`
- `data/processed/cleaned_Agriculture_price_dataset.csv`
- `data/processed/cleaned_Crop_recommendation.csv`
- `data/processed/cleaned_ICRISAT-District Level Data.csv`

## Troubleshooting

- If backend fails on startup, verify model files exist in `data/models/`
- If frontend shows API errors, ensure backend is running on port `5000`
- If live mandi data is empty, check API keys and commodity spelling

## Contribution Workflow

```bash
git checkout -b feature/your-feature
git add .
git commit -m "Describe changes"
git push origin feature/your-feature
```

## Next Release Focus

- Merge WhatsApp chatbot branch
- Merge Government Schemes information module
- Add branch-wise integration tests for new farmer support modules

---

Built for practical farmer decision support with ML + market intelligence.
