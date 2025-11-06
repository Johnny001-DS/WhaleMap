# 🐋 Satellite-Assisted Whale Migration Prediction

**Authors:** Sajan Arora & Karan Badlani  
**Institution:** Northeastern University, Data Science Program  
**Project Type:** Machine Learning for Ecological Forecasting  
**Phase:** I – Baseline & Spatial Modeling

---

## 🎯 Objective

Predict high-risk ocean regions where **North Atlantic right whales** are most likely to be present, using **NASA satellite data** and **WhaleMap detections**.  
These predictions can help ships and policy agencies reduce **ship-strike risks** for one of the world’s most endangered species (fewer than **370 whales remain**, with ~70 reproductive females).

---

## 🌊 Motivation

Ship collisions are a major cause of death for right whales.  
Traditional conservation approaches rely on manual sightings, which are **delayed and spatially sparse**.  
Our goal is to build a **data-driven early-warning system** that forecasts whale presence based on dynamic ocean conditions.

---

## 🛰️ Data Sources

| Dataset | Description | Source |
|----------|--------------|--------|
| **MODIS Aqua (SST)** | Sea Surface Temperature | NASA PO.DAAC |
| **MODIS Aqua (Chlorophyll-a)** | Surface chlorophyll concentration (proxy for plankton) | NASA OB.DAAC |
| **OSCAR Ocean Currents** | Surface current velocity (u, v) | NASA PO.DAAC |
| **Right Whale Sightings** | Presence detections | NOAA RWSAS / WhaleMap |
| **OBIS-SEAMAP** | Global whale records (for external validation) | OBIS / Duke University |

**Study Region:** Gulf of St. Lawrence  
**Temporal Resolution:** Weekly grids (~0.25°)  
**Total Samples:** ~819 weekly grid cells with presence/absence labels

---

## ⚙️ Data Preprocessing

1. Cropped NASA datasets to study area bounds.  
2. Aggregated daily satellite data to weekly means to reduce noise.  
3. Merged whale sightings with environmental grid cells by week.  
4. Added pseudo-absence samples for balance (~4.7% presence rate).  
5. Standardized all features (z-scores).  
6. Engineered **lag variables** (e.g., SST₍t-1₎, Chlorophyll₍t-1₎) to model temporal trends.

---

## 🧠 Feature Engineering

**Base Features**
- Latitude, Longitude  
- Sea Surface Temperature (SST)  
- Chlorophyll-a concentration  
- Ocean current components (u, v)  
- Derived current magnitude & direction  

**Derived Interaction Features**
- `sst2`, `chlorophyll2`  
- `sst_chl` (temperature × productivity)  
- `sst_u`, `chl_v` (current × environmental factors)

These capture **non-linear ecological responses** — for instance, whales preferring **cooler, plankton-rich waters** with moderate current flow.

---

## 🔍 Exploratory Data Analysis

- **SST vs. Chlorophyll:** weak negative correlation (cooler = more productive).  
- **Whale Presence vs. Chlorophyll:** mild positive correlation.  
- **Whale Presence vs. SST:** mild negative correlation.  
- **Currents:** weak but contextual influence (eastward flow association).  
- **KDE Plots:** whale detections cluster in moderately cool, high-chlorophyll regions.

---

## 🧪 Modeling Approach

We trained multiple supervised models to predict whale presence based on satellite-derived features:

| Model | Type | Highlights |
|--------|------|------------|
| **Logistic Regression** | Linear Baseline | interpretable, strong recall |
| **Random Forest** | Ensemble | captures non-linear effects |
| **XGBoost** | Gradient Boosting | optimized for precision-recall |

### Evaluation Metrics
- Accuracy  
- Precision / Recall / F1  
- ROC-AUC and PR-AUC  
- HitRate@Top-10% (ecological hotspot precision)

---

## ⚖️ Baseline Model – Logistic Regression

**Setup**
- 80/20 stratified train-test split  
- StandardScaler + class_weight="balanced"  
- Implemented via `scikit-learn` pipeline  

**Results**
- ROC-AUC ≈ **0.70**  
- PR-AUC ≈ **0.41**  
- Strong positive coefficient for **Chlorophyll**  
- Mild negative coefficient for **SST**

**Interpretation:**  
Chlorophyll is the dominant predictor — whales track productive feeding areas.  
Temperature and current variables provide secondary ecological context.

---

## 🌿 Insights

- **Chlorophyll** → strongest indicator (proxy for prey abundance).  
- **SST** → cooler waters preferred.  
- **Currents** → moderate contextual influence.  
- Satellite data alone can moderately predict whale presence, validating the approach.

---

## 🚧 Limitations

- Whale presence <5% → severe class imbalance.  
- WhaleMap sightings are biased by survey effort and weather.  
- 0.25° (~25 km) grid may miss finer ecological fronts.  
- Correlation ≠ causation — models infer patterns, not behavior.

---

## 🚀 Next Steps (Phase II)

- Integrate **acoustic detections** for real-time signals.  
- Add **spatio-temporal deep learning models** (ConvLSTM, 3D-CNN).  
- Expand to **Bay of Fundy** and **Cape Cod Bay** regions.  
- Deploy as an **interactive dashboard** for maritime authorities.

---

## 🧩 Tech Stack

- **Python 3.10**  
- **Libraries:** pandas, numpy, matplotlib, seaborn, scikit-learn, xgboost  
- **Data Sources:** NASA PO.DAAC / OB.DAAC, NOAA RWSAS, WhaleMap API  
- **Environment:** Jupyter Notebook / Google Colab  

---

## 📈 Key Results Summary

| Model | ROC-AUC | PR-AUC | Recall | Key Predictor |
|--------|----------|---------|---------|----------------|
| Logistic Regression | 0.70 | 0.41 | 0.74 | Chlorophyll |
| Random Forest | 0.78 | 0.63 | 0.68 | Chlorophyll × SST |
| XGBoost | 0.76 | 0.59 | 0.65 | Chlorophyll |

---

## 🗺️ Visualization (Phase I Output)

The model successfully reconstructs **known whale hotspots** in the Gulf of St. Lawrence using satellite features alone — demonstrating that ocean productivity and thermal gradients are sufficient to predict presence patterns.

---

## 🏁 Conclusion

This project demonstrates how **remote sensing + machine learning** can support marine conservation.  
By leveraging satellite-derived environmental data, we can move toward **real-time, data-driven whale protection systems** that reduce collision risks and preserve biodiversity.

---

## 📚 Citation

If you reference this work, please cite:

> Arora, S., & Badlani, K. (2025). *Satellite-Assisted Whale Migration Prediction.* Northeastern University, Data Science Program.

---
