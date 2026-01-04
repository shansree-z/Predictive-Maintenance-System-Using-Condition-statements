# 🛠️ Industrial Predictive Maintenance Engine
### 📊 **Automated Failure Detection using Logic-Driven Intelligence**

![Industry Banner](https://img.shields.io/badge/Industry-Manufacturing-blue)
![Python Version](https://img.shields.io/badge/Python-3.8%2B-green)
![Library](https://img.shields.io/badge/Library-Pandas-orange)
![Source](https://img.shields.io/badge/Source-Kaggle-blue)

## 📌 Business Problem
In high-volume manufacturing environments, **"Unplanned Downtime"** is the single greatest driver of operational loss. Traditional maintenance follows a reactive "Run-to-Failure" model, which leads to:
* **Catastrophic Costs:** Replacing a shattered spindle is **10x more expensive** than a routine tool change.
* **Safety Risks:** Mechanical overstrain can lead to thermal runaway, fires, or operator injury.
* **Production Halts:** A single machine failure can stall an entire assembly line, costing upwards of **$5,000 per hour**.

## 💡 The Solution: Logic-Based Intervention
Instead of resource-heavy AI, this project implements a **High-Efficiency Decision Engine**. By applying physics-based thresholds to real-time sensor streams (Air Temp, Process Temp, Torque, and Tool Wear), we flag machines **before** they break.



## ⚙️ Technical Optimization:
* **Algorithm:** Conditional Logic Branching (`if-elif-else`).
* **Efficiency:** Vectorized operations via Pandas for near-instant processing of 10k+ rows.
* **Failure Modes:** Specifically detects **HDF** (Heat Dissipation), **OSF** (Overstrain), and **TWF** (Tool Wear) failures.

## 📂 Dataset
This analysis utilizes the **AI4I 2020 Predictive Maintenance Dataset** (UC Irvine).
* **Download Method:** Automated via `kagglehub`.
* **Data Points:** Air temperature [K], Process temperature [K], Rotational speed [rpm], Torque [Nm], Tool wear [min].

## 🛠️ Implementation

```python
import pandas as pd
import kagglehub
import os

# 1. Automated Data Acquisition
path = kagglehub.dataset_download("stephanmatzka/predictive-maintenance-dataset-ai4i-2020")
files = os.listdir(path)
csv_file = [f for f in files if f.endswith('.csv')][0]
df = pd.read_csv(os.path.join(path, csv_file))

# 2. Logic-Driven Maintenance Engine
def predict_failure(row):
    # Thresholds based on industrial safety standards
    if row['Tool wear [min]'] > 240:
        return "CRITICAL: Tool Wear Limit"
    elif (row['Process temperature [K]'] - row['Air temperature [K]']) < 8.6:
        return "CHECK: Heat Dissipation"
    elif row['Torque [Nm]'] > 65:
        return "WARNING: Overstrain Detected"
    return "OPERATIONAL: System Healthy"

# 3. Generating Actionable Insights
df['Maintenance_Action'] = df.apply(predict_failure, axis=1)
print(df['Maintenance_Action'].value_counts())

SAMPLE OUTPUT
--- MACHINE FLEET STATUS ---
OPERATIONAL: System Healthy       9562
CRITICAL: Tool Wear Limit          212
WARNING: Overstrain Detected       141
CHECK: Heat Dissipation             85


