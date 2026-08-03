# 🏠 Boston House Price Prediction

A Flask web application that predicts median house prices in Boston using a trained **Linear Regression** model. Enter thirteen tract-level housing and demographic features through a clean web interface and receive an instant price estimate — also available as a JSON API for programmatic use.

<p>
  <img alt="Python" src="https://img.shields.io/badge/Python-3.9%2B-blue">
  <img alt="Flask" src="https://img.shields.io/badge/Flask-3.x-black">
  <img alt="scikit-learn" src="https://img.shields.io/badge/scikit--learn-1.6%2B-orange">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-green">
</p>

---

## Live Demo
 
🔗 **[http://housepricing-env.eba-6dmk8pq2.eu-north-1.elasticbeanstalk.com/](http://housepricing-env.eba-6dmk8pq2.eu-north-1.elasticbeanstalk.com/)**

---

## Deployment
 
This project is deployed on **AWS**, using the following pipeline:
 
- **AWS Elastic Beanstalk** — hosts and runs the Flask application
- **AWS CodePipeline** — automates build and deployment whenever code is pushed
- **AWS IAM** — manages permissions between CodePipeline and Elastic Beanstalk
Every push to the connected repository automatically triggers a new deployment through CodePipeline, which builds and releases the updated app to the Elastic Beanstalk environment.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Running the App](#running-the-app)
- [Usage](#-usage)
  - [Web Interface](#web-interface)
  - [REST API](#rest-api)
- [Input Features](#-input-features)
- [Model Details](#-model-details)
- [Troubleshooting](#-troubleshooting)
- [Future Improvements](#-future-improvements)
- [License](#-license)

---

## 🔍 Overview

This project serves a pre-trained regression model — built on the classic **Boston Housing dataset** — behind a Flask web server. Users can submit property and neighborhood characteristics via a form and receive a predicted median home value, or integrate directly with the `/predict_api` JSON endpoint.

---

## ✨ Features

- 🎯 Real-time price prediction from 13 input features
- 🌐 Simple, responsive web form (no JavaScript framework required)
- 🔌 JSON REST API for programmatic predictions
- ⚖️ Feature scaling applied automatically via a saved `StandardScaler`
- 🎨 Custom-styled UI (`static/style.css`)

---

## 📁 Project Structure

```
House_Pricing/
├── app.py                 # Flask application (routes + inference logic)
├── MLmodel.pkl             # Trained Linear Regression model (pickled)
├── scaler.pkl               # Fitted StandardScaler (pickled)
├── templates/
│   └── index.html          # Web form + result display
├── static/
│   └── style.css            # App styling
├── requirements.txt        # Python dependencies
└── README.md                 # Project documentation
```

---

## 🛠 Tech Stack

| Layer          | Technology              |
|----------------|--------------------------|
| Backend        | Python, Flask            |
| ML / Data      | scikit-learn, NumPy, pandas |
| Model          | Linear Regression         |
| Preprocessing  | StandardScaler             |
| Frontend       | HTML5, CSS3                |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9 or higher
- `pip` package manager

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/marwanelshahawy/House_Pricing.git
   cd House_Pricing
   ```

2. **Create and activate a virtual environment**
   ```bash
   python -m venv venv

   # Windows
   venv\Scripts\activate

   # macOS / Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

   If you don't have a `requirements.txt` yet, create one with:
   ```
   flask
   scikit-learn==1.6.1
   pandas
   numpy
   matplotlib
   seaborn
   gunicorn
   ```
   > ⚠️ Pin `scikit-learn` to the version used to train `MLmodel.pkl` / `scaler.pkl` to avoid `InconsistentVersionWarning` or prediction mismatches.

### Running the App

```bash
python app.py
```

The server starts in debug mode at:

```
http://127.0.0.1:5000
```

---

## 💻 Usage

### Web Interface

1. Navigate to `http://127.0.0.1:5000` in your browser.
2. Fill in all 13 fields (see [Input Features](#-input-features) below).
3. Click **Estimate value**.
4. The predicted price appears beneath the form.

### REST API

Send a `POST` request to `/predict_api` with a JSON body containing a `data` object of feature values.

**Endpoint:** `POST /predict_api`
**Content-Type:** `application/json`

**Example request:**
```bash
curl -X POST http://127.0.0.1:5000/predict_api \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "CRIM": 0.00632,
          "ZN": 18.0,
          "INDUS": 2.31,
          "CHAS": 0.0,
          "NOX": 0.538,
          "RM": 6.575,
          "AGE": 65.2,
          "DIS": 4.09,
          "RAD": 1.0,
          "TAX": 296,
          "PTRATIO": 15.3,
          "B": 396.9,
          "LSTAT": 4.98
        }
      }'
```

**Example response:**
```json
30.09
```

> ℹ️ Feature order in the request must match the order the scaler/model were trained on (see table below).

---

## 📊 Input Features

| Code      | Description                                     |
|-----------|--------------------------------------------------|
| `CRIM`    | Per-capita crime rate by town                    |
| `ZN`      | Proportion of residential land zoned for large lots |
| `INDUS`   | Proportion of non-retail business acres per town |
| `CHAS`    | Charles River dummy variable (1 if tract bounds the river, else 0) |
| `NOX`     | Nitric oxide concentration (parts per 10 million) |
| `RM`      | Average number of rooms per dwelling             |
| `AGE`     | Proportion of owner-occupied units built before 1940 |
| `DIS`     | Weighted distance to five Boston employment centers |
| `RAD`     | Index of accessibility to radial highways        |
| `TAX`     | Full-value property tax rate per $10,000         |
| `PTRATIO` | Pupil–teacher ratio by town                      |
| `B`       | Historic demographic index from the original dataset |
| `LSTAT`   | Percentage of lower-status population            |

---

## 🧠 Model Details

- **Algorithm:** Linear Regression (`sklearn.linear_model.LinearRegression`)
- **Preprocessing:** Features are standardized with a fitted `StandardScaler` before inference
- **Serialization:** Both the model and scaler are persisted with `pickle`
- **Target:** Median value of owner-occupied homes (in $1000s)

> The Boston Housing dataset is a widely used benchmark dataset in ML education. It has been deprecated from `scikit-learn` itself due to ethical concerns around the `B` feature; it remains useful here purely as a demonstration dataset for the end-to-end deployment workflow.

---

## 🧩 Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| `InconsistentVersionWarning` on load | `MLmodel.pkl` / `scaler.pkl` trained with a different scikit-learn version | Install the matching version, or retrain and re-pickle with your current version |
| `TypeError: Object of type ... is not JSON serializable` | Returning a raw NumPy array/scalar from `jsonify()` | Cast prediction to `float()` before returning |
| `ImportError: cannot import name 'app' from 'flask'` | Accidentally importing `app` from the `flask` package | Remove `app` from the `from flask import ...` line |
| Predictions look wrong | Form field order doesn't match the order used during training | Confirm the 13 features are submitted in the same order as the training data columns |

---

## 🔭 Future Improvements

- [ ] Add input validation and user-friendly error messages
- [ ] Add unit tests for `/predict` and `/predict_api`
- [ ] Add a `requirements.txt` with pinned versions (see above)
- [ ] Swap in a more robust model (e.g., Random Forest, Gradient Boosting) and compare performance

---

<p align="center">Built as part of an end-to-end machine learning deployment project.</p>