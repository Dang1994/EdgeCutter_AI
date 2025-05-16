# **AI Driven Shrink-Wrapper **  

## **📌 Project Overview**  
My aim in this project is to develop **predictive maintenance** tool for the **Vega shrink-wrapper**, a packaging machine used in the food and beverage industry. The machine groups bottles/cans, wraps them in plastic film, and heat-shrinks the film to form packages. A critical component is the **cutting blade**, which degrades over time but cannot be visually inspected during operation.  

My goal is to:  
✅ **Detect blade wear** using sensor data.  
✅ **Predict remaining useful life (RUL)** to prevent unexpected downtime.  
✅ **Classify operational modes** (8 modes) from time-series data.  

## **🔧 Problem Statement**  
### **Why is this important?**  
- The **cutting blade** is enclosed and operates at high speeds → **no visual inspection possible**.  
- **Blade failure** leads to production stoppages → costly downtime.  
- Current maintenance is **reactive** (fix after failure) or **preventive** (fixed schedules).  
- **Predictive maintenance** can optimize blade replacement, reducing costs.  

### **Challenges**  
- **Degradation trends** are hidden in high-frequency sensor data.  
- **8 operational modes** complicate analysis (different speeds/stresses).  
- Need to distinguish **normal wear vs. critical failure**.  

## **📊 Dataset Description**  
### **Source**  
- Collected from a real Vega shrink-wrapper over **12 months**.  
- Part of the **IMPROVE** EU Horizon 2020 project.  

### **Features**  
| Feature | Description | Relevance |
|---------|------------|-----------|
| `timestamp` | Time of recording (4ms intervals) | Time-series alignment |
| `pCut::Motor_Torque` | Torque on cutting motor | Indicates blade stress |
| `pCut::CTRL_Position_controller::Lag_error` | Blade position error | **Key degradation signal** |
| `pCut::CTRL_Position_controller::Actual_position` | Real-time blade position | Motion dynamics |
| `pCut::CTRL_Position_controller::Actual_speed` | Blade rotation speed | Wear rate factor |
| `pSvolFilm::CTRL_Position_controller::Actual_position` | Film feed position | Affects cut quality |
| `pSvolFilm::CTRL_Position_controller::Actual_speed` | Film feed speed | Consistency check |
| `pSvolFilm::CTRL_Position_controller::Lag_error` | Film positioning error | Secondary degradation signal |
| `pSpintor::VAX_speed` | Spindle speed | Machine load indicator |

### **File Naming Convention**  
Files are named as:  
`MM-DDTHHMMSS_NUM_modeX.csv`  
- `MM` = Month (1-12)  
- `DD` = Day  
- `X` = Operational mode (1-8)  

## **🛠️ Solution Approach**  
### **1. Exploratory Data Analysis (EDA)**  
- Visualize **degradation trends** (e.g., `Lag_error` vs. time).  
- Detect **blade replacement events** (sudden improvements).  
- Analyze **mode-specific behavior** (clustering).  

### **2. Feature Engineering**  
- **Rolling statistics** (mean, std of torque/lag error).  
- **Health score** = `1 - (normalized Lag_error)`.  
- **Frequency-domain features** (FFT for vibration analysis).  

### **3. Machine Learning Models**  
| Task | Model | Purpose |
|------|-------|---------|
| **Anomaly Detection** | Isolation Forest, Autoencoder | Flag abnormal blade behavior |
| **RUL Prediction** | LSTM, Random Forest | Estimate remaining blade life |
| **Mode Classification** | K-Means, DTW | Identify 8 operational modes |

### **4. Predictive Maintenance Deployment**  
- **Alert system** for early failure warnings.  
- **Maintenance scheduler** based on RUL predictions.  

## **📂 Repository Structure**  
```
├── /data/                  # Raw CSV files
├── /notebooks/             # Jupyter notebooks (EDA, modeling)
│   ├── 01_EDA.ipynb        # Exploratory analysis
│   ├── 02_Feature_Engineering.ipynb  
│   └── 03_Model_Training.ipynb  
├── /models/                # Trained models (pickle/h5)
├── /reports/               # Visualizations & insights
├── app.py                  # Demo Flask API (optional)
└── README.md               # This file
```


## **🚀 How to Run**  
1. **Install dependencies**:  
   ```bash
   pip install pandas numpy scikit-learn tensorflow matplotlib seaborn
   ```
2. **Run notebooks**:  
   - Start with `01_EDA.ipynb` for data exploration.  
   - Train models in `03_Model_Training.ipynb`.  

