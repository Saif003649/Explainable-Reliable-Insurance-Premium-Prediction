# Explainable & Reliable Insurance Premium Prediction System

An end-to-end Machine Learning web application built with **Flask**, **XGBoost**, and **SHAP** (SHapley Additive exPlanations). This system delivers transparent, uncertainty-aware health insurance premium estimations alongside interactive counterfactual "What-If" scenario simulations.

---

## 📌 Executive Overview

Traditional health insurance risk estimation models often operate as "black boxes," making it difficult for policyholders and underwriters to understand why a specific premium rate was quoted. 

This platform bridges that gap by combining:
1. **Accurate Gradient Boosted Predictions**: High-precision estimation powered by trained XGBoost regression models.
2. **Reliability & Uncertainty Bounds**: Dual point estimates alongside bounded 95% confidence intervals `[Lower Bound, Upper Bound]` to account for variance and statistical uncertainty.
3. **Transparent Explainability (XAI)**: Local and global SHAP attribution decomposing quotes into exact numerical factor weights (e.g., how much BMI or smoking status increased/decreased the bill).
4. **Interactive What-If Simulations**: Dynamic scenario playground enabling customers to test proactive lifestyle changes (e.g., quitting smoking, reducing BMI) and immediately see projected annual premium savings.

---

## 🌟 Key Features

- **Customer Profile Form (`/predict`)**: Comprehensive input interface capturing demographic and health markers (Age, Sex, BMI, Dependents, Smoking status, and Region) with validation and live preview.
- **Premium Prediction & Reliability Engine (`/result`)**: Displays estimated annual premium, point estimate, and bounded prediction intervals `[Lower 95%, Upper 95%]` alongside initial factor influences.
- **Explainability Center (`/explain`)**: In-depth SHAP waterfall visualizations and feature attribution tables revealing exact positive and negative pricing pressures for each patient profile.
- **What-If Scenario Simulator (`/whatif`)**: Interactive scenario controls comparing original customer baseline metrics against target lifestyle adjustments to compute prospective monetary savings in real-time.
- **Architectural Overview (`/about`)**: Detailed breakdown of system concepts, machine learning pipelines, uncertainty quantification formulas, and ethical AI considerations.

---

## 🏗️ Architecture & Tech Stack

```
+-------------------------------------------------------------+
|                        Frontend UI                          |
|         Jinja2 Templates, HTML5, CSS3, JavaScript           |
+------------------------------+------------------------------+
                               | HTTP Requests / Forms
+------------------------------v------------------------------+
|                     Flask Web Server (app.py)               |
|            Routing, Form Validation & Session Management     |
+---------------+-----------------------------+---------------+
                |                             |
+---------------v---------------+ +-----------v---------------+
|  What-If Engine               | |  ExplainAI Engine         |
|  (whatifanalysis.py)          | |  (ExplainAI.py)           |
|  - Point Predictions          | |  - Local/Global SHAP      |
|  - 95% Uncertainty Intervals  | |  - Waterfall Visualizer   |
|  - Counterfactual Delta       | |  - Factor Attribution     |
+---------------+---------------+ +-----------+---------------+
                \                             /
                 +-------------+-------------+
                               |
                +--------------v--------------+
                |    Artifacts (artifacts/)   |
                |       - model.pkl           |
                |    (Pipeline + XGBoost)     |
                +-----------------------------+
```

### Technology Breakdown
- **Web Framework**: Flask 3.x, Jinja2, Werkzeug
- **Core ML & Data Processing**: XGBoost 2.x, Scikit-Learn (ColumnTransformer, Pipelines), NumPy, Pandas
- **Explainable AI (XAI)**: SHAP 0.45+ (TreeExplainer, Waterfall plots)
- **Visualization**: Matplotlib, FontAwesome, Custom CSS Design System
- **Model Persistence**: Joblib, Pickle

---

## 📊 Data Dictionary & Supported Inputs

| Feature Name | Type | Allowed Range / Values | Description |
| :--- | :--- | :--- | :--- |
| **`age`** | Integer | `18` to `100` | Customer age in years |
| **`sex`** | Categorical | `male`, `female` | Biological gender of applicant |
| **`bmi`** | Float | `10.0` to `80.0` | Body Mass Index ($\text{kg/m}^2$) |
| **`children`**| Integer | `0` to `20` | Number of dependent children/dependents |
| **`smoker`** | Categorical | `yes`, `no` | Tobacco consumption status |
| **`region`** | Categorical | `northeast`, `northwest`, `southeast`, `southwest` | Residential geographic region in the US |

---

## 🎨 Color Palette & Design System

The frontend interface incorporates an accessible, trust-centric healthcare color palette:
- **Navy Blue (`#0b306b`)**: Primary brand elements, navigation headers, primary buttons.
- **Cornflower Blue (`#4f81b7`)**: Secondary accents, sliders, informative icons.
- **Freesia Accent (`#fdd8a5`)**: Highlights, demo badges, estimate focus containers.
- **Blue-Green / Emerald (`#16796f`)**: Success badges, savings indicators, positive healthcare impact.

---

## 🚀 Quickstart Guide

### 1. Prerequisites
- **Python 3.10+** (Tested on Python 3.10 – 3.13)
- **Pip** package manager
- **Git**

### 2. Environment Setup

Clone the repository and set up a virtual environment:

```bash


# Create virtual environment
python -m venv venv

# Activate on Windows (PowerShell)
.\venv\Scripts\Activate.ps1

# Activate on Windows (Command Prompt)
venv\Scripts\activate.bat

# Activate on Linux / macOS
source venv/bin/activate
```

### 3. Install Dependencies

Ensure your package manager is up to date and install required dependencies:

```bash
pip install -r requirements.txt
```

### 4. Launch Application

Start the Flask development server:

```bash
python app.py
```

### 5. Open in Browser

Open your preferred browser and navigate to:
```
http://127.0.0.1:5000
```

---

## 📁 Project Structure

```
insurance_ui_demo/
│
├── app.py                  # Main Flask application, routing, and controller logic
├── ExplainAI.py            # SHAP TreeExplainer, waterfall generator & factor parser
├── whatifanalysis.py       # Point prediction, uncertainty bounds, and what-if engine
├── requirements.txt        # Production Python dependencies
├── README.md               # Project documentation
├── .gitignore              # Git ignore configuration
│
├── artifacts/              # Pretrained models and machine learning pipelines
│   └── model.pkl           # Pickled model pipeline (ColumnTransformer + XGBoost)
│
├── templates/              # Jinja2 HTML layout and page templates
│   ├── base.html           # Master layout with top-nav, sidebar, and alerts
│   ├── landing.html        # Platform landing and feature introduction page
│   ├── predict.html        # Customer profile intake form with validation
│   ├── result.html         # Premium prediction, uncertainty range & factor summary
│   ├── explain.html        # Detailed SHAP waterfall and factor attribution analysis
│   ├── whatif.html         # Interactive counterfactual scenario simulator
│   └── about.html          # Conceptual documentation and mathematical overview
│
└── static/                 # Static assets
    ├── css/
    │   └── style.css       # Unified design system and component styling
    ├── js/
    │   ├── main.js         # Navigation, toast notifications, UI loading state
    │   └── demo.js         # Scenario calculations, sliders, and chart hooks
    └── images/
        └── shap/           # Runtime directory for generated SHAP waterfall plots
```

---

## 🛡️ Model Compatibility & Version Notes

The provided serialized model (`artifacts/model.pkl`) was exported with Scikit-learn. To ensure seamless backwards compatibility across environments running Scikit-learn 1.6 through 1.7+, both `ExplainAI.py` and `whatifanalysis.py` implement an automated compatibility patch for `_RemainderColsList`, preventing unpickling errors on modern Python and Scikit-learn distributions.

---

## 📄 License

This project is released under the MIT License. Feel free to use, modify, and extend for research and production workflows.
