<div align="center">

# ⛽💧 E20 Fuel Phase Separation Predictor

**An Industrial IoT digital twin and machine learning dashboard that forecasts water ingress and warns operators before phase separation happens in E20 underground storage tanks.**

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![XGBoost](https://img.shields.io/badge/Model-XGBoost-orange)
![Streamlit](https://img.shields.io/badge/UI-Streamlit-red)
![Plotly](https://img.shields.io/badge/Charts-Plotly-3F4F75)

</div>

---

## 📑 Table of Contents

- [Background](#-background)
- [The Solution](#-the-solution)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Using the Dashboard](#-using-the-dashboard)
- [Alert Levels](#-alert-levels)
- [Troubleshooting](#-troubleshooting)
- [Future Scope](#-future-scope)

---

## 📖 Background

India is moving rapidly to **E20** fuel (20% ethanol, 80% petrol) to improve energy security and cut carbon emissions. This shift creates a serious logistics and chemical engineering problem: **phase separation**.

- Ethanol is **hygroscopic**: it absorbs water from the atmosphere.
- Underground tanks at retail outlets (OMCs) go through **diurnal temperature swings**. The tank "breathes" and draws in humid air.
- When water content in E20 passes a critical threshold (about **0.5%, or 5,000 ppm**), the ethanol-water mixture separates from the petrol and sinks to the bottom of the tank.
- The result is unusable fuel and potential damage to vehicle engines.

Today, operators check for water by **manual tank dipping** several times a day. This project automates that process by **predicting separation before it happens**.

---

## 🎯 The Solution

This repository contains an ML pipeline and an interactive Streamlit dashboard that act as a **digital twin** of an underground storage tank.

Real-time environmental telemetry (temperature swings, ambient humidity, tank vapor space) feeds an **XGBoost regressor**. The model forecasts the hourly rate of moisture ingress and produces a **72-hour predictive curve**. If the tank is on course to cross the 5,000 ppm limit, the dashboard raises an alert.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| **Physics-informed synthetic data** | Simulates thermodynamic tank behavior, including diurnal breathing cycles and moisture accumulation. |
| **IoT sensor simulation** | Generates a continuous, rolling 14-day feed of tank telemetry. |
| **ML forecasting engine** | A trained XGBoost model predicts hourly and daily water ingress velocity from current conditions. |
| **Interactive UI** | Plotly and Streamlit stitch 14 days of historical actuals to a high-resolution 3-day forecast. |
| **Automated alerting** | Indicators move from Safe (green) to Warning (yellow) to Imminent Phase Separation (red). |
| **Live stream simulation** | Optional animated mode that adds a new reading every second. |
| **Manual override** | Sliders for temperature, humidity and vapor space show the time to phase separation instantly. |

---

## ⚙️ How It Works

```text
generate_data.py ──► data/synthetic_tank_data.csv ──► train_model.py ──► models/xgb_model.json
                                                                              │
generate_sensor_log.py ──► data/sensor_log.csv ──────────────────────────────►│
                                                                              ▼
                                                                    app.py (Streamlit)
                                                         history + 72-hour forecast + alerts
```

1. **Data generation:** `generate_data.py` builds a physics-informed training dataset.
2. **Training:** `train_model.py` fits an XGBoost regressor and exports it to `models/xgb_model.json`.
3. **Sensor simulation:** `generate_sensor_log.py` creates a rolling 14-day telemetry log.
4. **Dashboard:** `app.py` loads the log and the model, forecasts 72 hours ahead, and shows alerts.

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| Language | Python 3.8+ |
| Machine learning | XGBoost, Scikit-Learn |
| Data handling | Pandas, NumPy |
| Dashboard | Streamlit |
| Visualization | Plotly Express |

---

## 📂 Project Structure

```text
E20_Predictor/
│
├── data/
│   ├── synthetic_tank_data.csv   # Base thermodynamic dataset for training
│   └── sensor_log.csv            # Rolling 14-day simulated IoT feed
│
├── models/
│   └── xgb_model.json            # Trained XGBoost regressor weights
│
├── app.py                        # Streamlit dashboard application
├── generate_data.py              # Generates training data
├── train_model.py                # Trains and exports the ML model
├── generate_sensor_log.py        # Simulates live IoT telemetry
├── requirements.txt              # Python dependencies
└── README.md                     # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or newer
- `git` and `pip`

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/E20_Predictor.git
cd E20_Predictor
```

### 2. Create a virtual environment (recommended)

**Windows (PowerShell):**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the ML pipeline

The data must exist and the model must be trained before the UI will work. Run these **in order**:

```bash
python generate_data.py          # 1. build training data
python train_model.py            # 2. train and export the XGBoost model
python generate_sensor_log.py    # 3. create the 14-day sensor feed
```

### 5. Launch the dashboard

```bash
streamlit run app.py
```

A browser tab opens automatically, usually at <http://localhost:8501>.

---

## 🎮 Using the Dashboard

| Mode | What it does |
|---|---|
| **Live Sensor Feed** | Reads `sensor_log.csv`, shows the 14-day history and projects a 3-day forecast. |
| **Simulate Real-Time Data Stream** | Tick the sidebar checkbox to simulate an active IoT connection. The chart animates with a new reading every second. |
| **Manual Override** | Use the sidebar sliders (Temperature, Humidity, Vapor Space) to test extreme conditions and see the effect on Time-to-Phase-Separation. |

---

## 🚦 Alert Levels

| Status | Color | Meaning |
|---|---|---|
| **Safe** | 🟢 Green | Water content is well below the 5,000 ppm limit. |
| **Warning** | 🟡 Yellow | Forecast is trending toward the limit. Plan an inspection. |
| **Imminent Phase Separation** | 🔴 Red | Forecast crosses 5,000 ppm. Act now. |

---

## 🧰 Troubleshooting

| Problem | Fix |
|---|---|
| `FileNotFoundError` for a CSV or model file | Run the three pipeline scripts in order (step 4) from the project root. |
| `ModuleNotFoundError` | Activate the virtual environment, then run `pip install -r requirements.txt`. |
| PowerShell blocks `Activate.ps1` | Run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned`, then activate again. |
| Port 8501 already in use | Run `streamlit run app.py --server.port 8502`. |
| Stale or odd forecasts | Delete `data/` and `models/` contents and rerun the pipeline. |

---

## 🔮 Future Scope

- [ ] Integrate real Raman spectroscopy or photothermal sensors over MQTT.
- [ ] Adjust the 5,000 ppm limit dynamically based on local fuel temperature drops.
- [ ] Deploy to AWS EC2 or Streamlit Community Cloud for remote depot monitoring.

---

<div align="center">

Built to keep E20 fuel clean, dry and dependable. ⛽

</div>