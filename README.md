# **NIRVANA_OIL: AI-Powered Oil Spill and Anomaly Detection System**  
## Created By Revanta Biswas And Srishti Gupta 
## **Detailed Workflow for Nirvana**  

Nirvana is an advanced maritime monitoring system that combines **AIS data** and **satellite SAR imagery** to detect **anomalies in vessel behavior** and **oil spills**. The system now incorporates AI agents for **enhanced anomaly detection, decision-making, and automated reporting**.  

---

## **1. Data Collection and Ingestion**  

### **a) Live AIS Data Collection**  
- **Source**: Real-time AIS data from vessels via AISHUB.  
- **Ingestion Mechanism**:  
  - **Apache Kafka** for real-time data streaming and preprocessing.  

### **b) Satellite SAR Data Collection**  
- **Source**: Sentinel-1 SAR satellite images.  
- **Ingestion Mechanism**:  
  - Periodic retrieval from Copernicus Open Access Hub.  

---

## **2. Preprocessing and Feature Extraction**  

### **a) AIS Data Preprocessing**  
- **Noise Removal**: Eliminates incomplete or erroneous messages.  
- **Feature Engineering**: Derives speed, acceleration, and course change metrics.  
- **Segmentation**: Breaks AIS data into vessel-specific time or spatial zones.  

### **b) Satellite SAR Image Preprocessing**  
- Standard SAR processing techniques applied before ML analysis.  

---

## **3. AI-Powered Anomaly Detection & Oil Spill Analysis**  

### **a) AIS Anomaly Detection**  
🔹 **AIS Anomaly Detection Agent** (NEW) — Works alongside existing models:  
1. **DBSCAN**: Detects anomalous vessel trajectories.  
2. **Kalman Filter**: Predicts expected movement patterns.  
3. **Isolation Forest**: Identifies outliers in vessel behavior.  
4. **Autoencoder**: Learns normal AIS patterns and flags deviations.  

### **b) Oil Spill Detection from Satellite SAR Images**  
🔹 **Satellite Image Processing Agent** (NEW) — Collaborates with ML models:  
1. **ResNet50**: Classifies SAR image patches as “Oil Spill” or “No Oil Spill.”  
2. **U-Net**: Segments oil spill regions in SAR images.  
3. **YOLOv5**: Detects and localizes oil spills and nearby vessels.  

---

## **4. Cross-Validation and Decision Making**  

🔹 **Decision-Making Agent** (NEW) — Positioned between AIS and oil spill detection models.  
- **Function**:  
  - **Cross-validates** AIS anomalies with satellite data.  
  - **Assigns priority levels** based on spill severity and vessel behavior.  
  - **Filters false positives** before alert generation.  

🔹 **CrewAI Decision Layer** (NEW) — Added between "Anomaly Detected and Reported" & "Oil Spill Detected and Reported."  
- Ensures AI-driven validation before oil spill confirmations.  

---

## **5. Automated Alerts, Reports & Feedback Optimization**  

🔹 **Alert & Reporting Agent** (NEW) — Added before "Report Generated."  
- **Function**:  
  - Automates **real-time alerts** for detected spills.  
  - Generates **structured reports** with vessel and spill details.  

🔹 **AI Response & Action Module** (NEW) — Positioned after "Oil Spill Detected and Reported."  
- **Function**:  
  - Automates reporting workflows and next actions for regulatory bodies.  

🔹 **AI Model Optimization Feedback** (NEW) — **New Feedback Loop** from "Report Generated" to "Pre-processing and Feature Selection."  
- **Function**:  
  - Continuously improves ML models based on past detection performance.  

---

## **6. Reporting and Dashboard Visualization**  

1. **Real-Time Monitoring**  
   - Live AIS map with **vessel positions, detected anomalies, and oil spills**.  

2. **AI-Powered Alerts**  
   - **Automated notifications** when anomalies or spills are detected.  

3. **Comprehensive Reports**  
   - Includes **AIS anomaly summaries**, **satellite spill detection**, and **cross-validation results**.  

---

## **7. System Components & Architecture**  

- **Dashboard**: Next.js frontend for real-time monitoring.  
- **Backend Services**: FastAPI for ML model integration.  
- **Data Pipelines**: Kafka streams for real-time AIS data.  
- **Storage**: PostgreSQL/MongoDB for logs, detections, and reports.  

---
.

### Software Architecture:
The image provided illustrates the overall architecture and data flow of Nirvana. It combines various machine learning methods, data pipelines, and visualization tools, enabling a cohesive monitoring system.

## Repository Structure
Here's a detailed file structure and description of each component in the Nirvana repository:

```
Nirvana
├── Dashboard
│   ├── prototype
│   │   ├── dashboard
│   │   │   ├── node_modules          # Dependencies for the web-based dashboard
│   │   │   └── src                   # Source code for dashboard implementation
│   │   ├── package.json              # Node.js package file
│   │   └── README.md                 # Documentation for the dashboard component
├── ML
│   ├── app
│   │   ├── app
│   │   │   ├── anomaly_detection_app_demo.py  # Main script for running anomaly detection
│   │   │   ├── models                 # Directory for machine learning models
│   │   │   │   └── model_files        # Model weights and configuration files
│   │   └── __init__.py                # Init file for the ML application
│   ├── models
│   │   ├── oil_spill_detection_model  # Models for detecting oil spills from SAR data
│   │   └── ais_anomaly_detection_model # Models for detecting AIS anomalies
│   ├── Notebooks
│   │   ├── Feature_Engineering.ipynb  # Jupyter notebook for feature engineering from AIS data
│   │   └── Model_Training.ipynb       # Notebook for training and evaluating ML models
│   └── README.md                      # Documentation for the ML components
├── Data
│   ├── AIS                            # Directory for storing pre-processed AIS data
│   ├── SAR                            # Directory for storing SAR satellite images
│   └── README.md                      # Information on how to use and structure the data
├── Report_Generation
│   ├── templates                      # Report templates and generation scripts
│   └── generate_report.py             # Script for generating reports
├── README.md                          # Main README file
└── .gitignore                         # Git ignore file
```

### Key Files and Directories:
- **Dashboard**: Contains the code for the interactive dashboard and client interface, allowing users to visualize AIS data and detected anomalies.
- **ML**: Contains machine learning models and scripts for both AIS and satellite data processing.
- **Notebooks**: Jupyter notebooks for experimenting with feature engineering, model training, and evaluation.
- **Data**: Structure for storing AIS and SAR satellite datasets.
- **Report_Generation**: Scripts and templates for generating detailed reports of detected anomalies and oil spills.

## How to Run
1. **Prerequisites**:
   - Python 3.7 or above
   - Node.js for the dashboard
   - Required Python libraries (listed in `requirements.txt`)
   - Docker (for containerized deployment)

2. **Step-by-Step Setup**:
   - Clone the repository:
     ```bash
     git clone https://github.com/username/Nirvana.git
     cd Nirvana
     ```
   - Set up the virtual environment and install dependencies:
     ```bash
     python -m venv venv
     source venv/bin/activate
     pip install -r requirements.txt
     ```
   - Install the dashboard dependencies:
     ```bash
     cd Dashboard/prototype
     npm install
     ```
   - Run the AIS anomaly detection service:
     ```bash
     python ML/app/app/anomaly_detection_app_demo.py
     ```
   - Start the dashboard:
     ```bash
     npm start
     ```
   - View the application in your browser at `http://localhost:3000`.



--- 

## Conclusion
Nirvana integrates machine learning models, statistical methods, and advanced image processing techniques to provide a holistic monitoring solution for maritime safety and environmental protection. With detailed workflows and robust validation mechanisms, the system offers reliable and timely detection of oil spills and vessel anomalies, empowering stakeholders with actionable insights. 
