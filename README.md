E20 Fuel Phase Separation Predictor ⛽💧

An Industrial IoT (IIoT) digital twin and machine learning dashboard built to forecast water ingress and prevent phase separation in E20 (20% ethanol) underground storage tanks.

📖 Project Background

India is rapidly transitioning to E20 fuel (a blend of 20% ethanol and 80% petrol) to enhance energy security and reduce carbon emissions. However, this shift introduces a critical logistical and chemical engineering bottleneck: Phase Separation.

Ethanol is highly hygroscopic (it absorbs water from the atmosphere). During logistical transport and underground storage at retail outlets (OMCs), fuel tanks undergo diurnal temperature swings (day/night heating and cooling). This causes the tanks to "breathe," drawing in humid atmospheric air. If the water content in an E20 blend exceeds a critical tolerance threshold (approximately 0.5% or 5,000 ppm), the ethanol-water mixture separates from the petrol and sinks to the bottom of the tank, rendering the fuel unusable and severely damaging vehicle engines.

Currently, OMC operators rely on manual tank dipping multiple times a day to check for water. This project automates and modernizes that process by predicting phase separation before it happens.

🎯 The Solution: Predictive Digital Twin

This repository contains a machine learning pipeline and an interactive Streamlit dashboard that acts as a "Digital Twin" for an underground storage tank.

By feeding real-time environmental telemetry (temperature swings, ambient humidity, and tank vapor space) into an XGBoost Regressor, the system accurately forecasts the hourly rate of moisture ingress. It provides operators with a 72-hour predictive curve, triggering automated alerts if a tank is on trajectory to breach the 5,000 ppm safety limit.

✨ Key Features

Physics-Informed Synthetic Data: Simulates accurate thermodynamic tank behavior (diurnal breathing cycles and moisture accumulation).

IoT Sensor Simulation: Generates a continuous, rolling 14-day historical feed of tank telemetry.

ML Forecasting Engine: Utilizes a trained XGBoost model to predict future daily/hourly water ingress velocities based on current environmental constraints.

Interactive UI (Plotly & Streamlit): Seamlessly stitches 14 days of historical actuals with a high-resolution 3-day predictive forecast curve.

Automated Alerting Protocol: Dynamic UI indicators that shift from Safe (Green) to Warning (Yellow) to Imminent Phase Separation (Red).

🛠️ Tech Stack

Core Language: Python 3.8+

Machine Learning: XGBoost, Scikit-Learn

Data Manipulation: Pandas, NumPy

Frontend / Dashboard: Streamlit

Visualization: Plotly Express

📂 Project Architecture

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
├── generate_data.py              # Script to generate training data
├── train_model.py                # Script to train & export the ML model
├── generate_sensor_log.py        # Script to simulate live IoT telemetry
├── requirements.txt              # Python dependencies
└── README.md                     # Project documentation


🚀 How to Clone and Run Locally

Follow these exact steps to get the Digital Twin running on your local machine.

1. Clone the Repository

Open your terminal or command prompt and run:

git clone https://github.com/YOUR_USERNAME/E20_Predictor.git
cd E20_Predictor


2. Set Up a Virtual Environment (Recommended)

Isolate your project dependencies by creating a virtual environment:

For Windows (PowerShell):

python -m venv venv
.\venv\Scripts\Activate.ps1


For macOS/Linux:

python3 -m venv venv
source venv/bin/activate


3. Install Dependencies

With your virtual environment active, install the required Python libraries:

pip install -r requirements.txt


4. Execute the ML Pipeline

You must build the data and train the model before launching the UI. Run these scripts sequentially:

Generate the training data:

python generate_data.py


Train the XGBoost model:

python train_model.py


Generate the historical sensor feed:

python generate_sensor_log.py


5. Launch the Dashboard

Start the Streamlit server:

streamlit run app.py


A browser window will automatically open (usually at http://localhost:8501) displaying the live interactive dashboard.

🎮 How to Use the Dashboard

Live Sensor Feed Mode: The dashboard will automatically read the sensor_log.csv file, display the 14-day history, and project a 3-day forecast.

Simulate Real-Time Data Stream: Check the box in the sidebar to simulate an active IoT connection. The chart will animate, generating new readings every second.

Manual Override: Select this mode from the sidebar to play with the sliders (Temperature, Humidity, Vapor Space) and instantly see how different environmental extremes affect the Time-to-Phase-Separation.

🔮 Future Scope

Integration with real-world Raman Spectroscopy or Photothermal sensors via MQTT protocols.

Adjusting the 5,000 ppm limit dynamically based on localized fuel temperature drops.

Deploying the dashboard to a cloud provider (AWS EC2 / Streamlit Community Cloud) for remote OMC depot monitoring.