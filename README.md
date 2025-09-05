# CareGuard AI - Chronic Care Risk Prediction Engine

🏥 **AI-Driven Risk Prediction Engine for Chronic Care Patients**

CareGuard AI predicts if chronic care patients (diabetes, heart failure, hypertension) will deteriorate in the next 90 days using routine EMR data. It provides clinician-friendly explanations and actionable recommendations to help reduce avoidable hospitalizations.

## 🎯 Overview

**Problem**: Chronic patients often deteriorate silently between visits, leading to preventable hospitalizations and poor outcomes.

**Solution**: CareGuard AI uses machine learning to identify high-risk patients early, with transparent explanations and specific clinical recommendations.

**Impact**: Enable proactive interventions, reduce emergency admissions, and improve chronic disease management.

## ⚡ Quick Start

### 1. Setup Environment

```shellscript
# Create virtual environment
python -m venv .venv

# Activate environment
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Generate Data & Train Model

```shellscript
# Generate synthetic patient data
cd src
python synth_data.py

# Train the risk prediction model
python train.py

# Test explainability
python explain.py
```

### 3. Run the Dashboard

```shellscript
# Launch Streamlit dashboard
cd ../app
streamlit run streamlit_app.py
```

### 4. Start API Server (Optional)

```shellscript
# Start FastAPI server
cd ../src
uvicorn api:app --reload --port 8000
```

## 🏗️ Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Data Sources  │    │   Feature Store  │    │   ML Pipeline   │
│                 │────│                  │────│                 │
│ • Vitals        │    │ • Engineered     │    │ • XGBoost       │
│ • Labs          │    │   Features       │    │ • Calibration   │
│ • Medications   │    │ • Time Windows   │    │ • Validation    │
│ • Adherence     │    │ • Risk Flags     │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                          │
                               ┌──────────────────────────┘
                               │
        ┌─────────────────┐    │    ┌─────────────────┐
        │  Explainability │    │    │   API Service   │
        │                 │────┼────│                 │
        │ • SHAP Values   │    │    │ • FastAPI       │
        │ • Clinical Text │    │    │ • Predictions   │
        │ • Recommendations│   │    │ • Explanations  │
        └─────────────────┘    │    └─────────────────┘
                               │              │
        ┌─────────────────┐    │              │
        │   Dashboard     │────┘              │
        │                 │───────────────────┘
        │ • Cohort View   │
        │ • Patient Detail│
        │ • Risk Analytics│
        └─────────────────┘
```

## 📊 Model Performance

- **AUROC**: 0.85+ (Excellent discrimination)
- **AUPRC**: 0.70+ (Good precision-recall balance)
- **Risk Bands**: Low (<10%), Medium (10-25%), High (>25%)
- **Calibration**: Isotonic calibration for reliable probabilities

## 🎯 Key Features

### 🔮 Risk Prediction
- **90-day deterioration risk** using routine clinical data
- **Three risk bands** with calibrated probabilities
- **Real-time scoring** through API or dashboard

### 🔍 Explainable AI
- **SHAP-based explanations** for model decisions
- **Clinical summaries** in plain language
- **Top risk drivers** with impact assessment

### 💡 Clinical Recommendations
- **Evidence-based actions** based on risk factors
- **Immediate, short-term, and long-term** interventions
- **Care coordination** suggestions

### 📈 Interactive Dashboard
- **Cohort overview** with filtering and sorting
- **Individual patient** drill-down views
- **Risk analytics** and population insights
- **What-if scenarios** for intervention planning

## 📁 Project Structure

```
careguard-ai/
├── data/
│   ├── raw/                 # Raw data files
│   └── processed/           # Processed training data
├── src/
│   ├── synth_data.py       # Synthetic data generation
│   ├── features.py         # Feature engineering
│   ├── train.py            # Model training pipeline
│   ├── explain.py          # SHAP explainability
│   ├── evaluate.py         # Model evaluation
│   ├── api.py              # FastAPI backend
│   └── utils.py            # Utility functions
├── app/
│   └── streamlit_app.py    # Streamlit dashboard
├── models/
│   ├── model.pkl           # Trained model bundle
│   └── metrics.json        # Performance metrics
├── notebooks/              # Jupyter notebooks
├── requirements.txt        # Python dependencies
└── README.md              # This file
```

## 🔬 Data Features

### Patient Demographics
- Age, sex, primary chronic condition

### Clinical Measurements  
- **HbA1c**: Diabetes control (last value)
- **Weight trend**: kg change over 30 days
- **Blood pressure**: Latest systolic reading
- **BNP**: Heart failure biomarker
- **eGFR trend**: Kidney function change over 90 days
- **BMI**: Body mass index

### Behavioral Factors
- **Medication adherence**: 30-day average
- **Care engagement**: Days since last lab
- **Smoking status**: Current smoking

### Engineered Features
- **Risk flags**: Binary indicators for clinical thresholds
- **Interaction terms**: Age-adherence, diabetes control score
- **Composite scores**: Metabolic risk, cardiac risk

## 🏥 Clinical Use Cases

### Primary Care
- **Population health management** - identify high-risk patients
- **Care coordination** - prioritize interventions
- **Preventive care** - proactive outreach

### Chronic Disease Management
- **Diabetes care** - HbA1c and adherence monitoring
- **Heart failure** - early decompensation detection
- **Hypertension** - medication optimization

### Care Teams
- **Nurse care managers** - patient prioritization
- **Pharmacists** - adherence interventions
- **Specialists** - referral optimization

## 🛡️ Safety & Ethics

### Clinical Safety
- **Decision support**, not replacement
- **Uncertainty quantification** and confidence intervals
- **Human-in-the-loop** design with clinician oversight

### Data Privacy
- **Synthetic data** for development and demos
- **HIPAA-compliant** architecture design
- **Minimal data collection** - only necessary features

### Bias & Fairness
- **Subgroup analysis** by age, sex, and condition
- **Performance monitoring** across demographics
- **Bias detection** and mitigation strategies

### Model Governance  
- **Version control** for models and data
- **Performance monitoring** in production
- **Regular retraining** and validation

## 📈 API Documentation

### Prediction Endpoints

```http
POST /predict
Content-Type: application/json

{
  "age": 65,
  "sex": "M",
  "condition_primary": "Diabetes",
  "hba1c_last": 8.5,
  "weight_trend_30d": 2.0,
  "adherence_mean": 0.75,
  "bnp_last": 200,
  "egfr_trend_90d": -5.0,
  "sbp_last": 150,
  "bmi": 32.0,
  "days_since_last_lab": 120,
  "smoker": 1
}
```

```http
POST /explain
# Same input format as /predict
# Returns detailed SHAP explanations
```

### Response Format

```json
{
  "risk_probability": 0.35,
  "risk_band": "High",
  "clinical_summary": "Rising HbA1c (8.5%) and low medication adherence (75%) increase deterioration risk.",
  "recommendations": [
    "Consider therapy intensification; recheck HbA1c in 30-45 days",
    "Enroll in adherence support program"
  ]
}
```

## 🧪 Testing

### Unit Tests
```shellscript
# Run feature engineering tests
python -m pytest tests/test_features.py

# Run model tests  
python -m pytest tests/test_models.py
```

### Integration Tests
```shellscript
# Test API endpoints
python -m pytest tests/test_api.py

# Test end-to-end pipeline
python -m pytest tests/test_pipeline.py
```

### Model Validation
```shellscript
# Comprehensive model evaluation
cd src
python evaluate.py
```

## 🚀 Deployment

### Local Development
```shellscript
# Start all services
docker-compose up -d
```

### Production Deployment
```shellscript
# Build Docker images
docker build -t careguard-ai:latest .

# Deploy to Kubernetes
kubectl apply -f k8s/
```

### Environment Variables
```shellscript
export MODEL_PATH="/app/models/model.pkl"
export API_HOST="0.0.0.0"
export API_PORT="8000"
export LOG_LEVEL="INFO"
```

## 📊 Performance Monitoring

### Model Metrics
- **Discrimination**: AUROC, AUPRC
- **Calibration**: Brier score, calibration plots
- **Clinical utility**: Number needed to screen, alert precision

### Operational Metrics  
- **API latency**: <100ms p95
- **Throughput**: 1000+ predictions/minute
- **Uptime**: 99.9% availability

### Data Quality
- **Feature drift** detection
- **Missing data** monitoring  
- **Outlier** identification

## 🤝 Contributing

### Development Setup
```shellscript
# Clone repository
git clone <repository-url>
cd careguard-ai

# Setup development environment
pip install -r requirements-dev.txt
pre-commit install
```

### Code Style
- **Black** for Python formatting
- **flake8** for linting
- **Type hints** with mypy
- **Docstrings** in Google style

### Pull Request Process
1. Fork the repository
2. Create feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit pull request with description

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

### Documentation
- **API Docs**: http://localhost:8000/docs (when running locally)
- **Model Card**: Available in `/models/model_card.md`
- **Technical Specs**: See `/docs/technical_specification.md`

### Contact
- **Issues**: GitHub Issues for bug reports and feature requests
- **Discussions**: GitHub Discussions for questions and ideas
- **Email**: [support@careguard-ai.com](mailto:support@careguard-ai.com)

---

**CareGuard AI** - Empowering clinicians with AI-driven insights for better chronic care outcomes 🏥✨
