# NASA C-MAPSS Engine Remaining Useful Life (RUL) Predictor

**Author:** Deevi Eswar  
**Live App:** [Link to your Streamlit App]

## What I Built
An end-to-end predictive maintenance dashboard that estimates the Remaining Useful Life (RUL) of turbofan engines using the NASA C-MAPSS dataset. Instead of relying on black-box deep learning (like LSTMs), I built a highly interpretable machine learning pipeline using **XGBoost, Scikit-learn, and Pandas**, deployed as an interactive **Streamlit** web application. 

The application allows users (like maintenance engineers) to input the last 30 operational cycles of engine sensor data and instantly receive an RUL prediction, helping aviation operations reduce unscheduled maintenance.

## What Broke and What I Figured Out
While building this, I initially fed all 21 raw sensor readings directly into a Random Forest model. The resulting RMSE was terrible, and the model was overfitting heavily. I spent hours debugging and finally realized that several sensors (like fan inlet temperature and bypass duct pressure) were completely flat-lined across all cycles—they were adding pure noise.

**The Fix:** I dropped the invariant sensors entirely and engineered new temporal features: specifically, a 30-cycle rolling mean and standard deviation for the remaining sensors to capture the degradation trend over time. This single data-engineering step dropped the RMSE significantly and allowed a simpler XGBoost model to outperform my initial complex setups. 

Additionally, handling state in Streamlit to simulate "live" sensor ingestion kept resetting the UI. I had to learn how to properly decouple the prediction logic from the rendering loop using `st.session_state` to make the dashboard feel responsive.

## Tech Stack
- **Modeling:** XGBoost, Scikit-Learn, Pandas, NumPy
- **Frontend/Deployment:** Streamlit, Streamlit Cloud
- **Domain:** Predictive Maintenance, Aerospace Operations

## How to Run Locally
1. Clone the repository: `git clone https://github.com/Eswar809/CMAPSS-RUL-Predictor.git`
2. Install dependencies: `pip install -r requirements.txt`
3. Run the app: `streamlit run myapp.py`
