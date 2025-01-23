# NIRVANA_OIL
# By Revanta Biswas and Srishti Gupta
# Detailed Workflow for Nirvana: Oil Spill and Anomaly Detection System

Nirvana is designed to monitor maritime environments using both AIS data and satellite Synthetic Aperture Radar (SAR) imagery. This workflow dives into the detailed steps involved in processing and analyzing these datasets, describing how each model and algorithm is used to detect anomalies and oil spills.

---

## 1. **Data Collection and Ingestion**

### a) **Live AIS Data Collection**
- **Source**: AIS data is streamed in real-time from vessels using the AISHUB.
- **Ingestion Mechanism**: 
  - **Apache Kafka** is used for real-time data ingestion and streaming. The Kafka server receives the AIS messages from various vessels, normalizes them, and forwards them to the preprocessing module.
  
### b) **Satellite SAR Data Collection**
- **Source**: Sentinel-1 SAR satellite data is used for high-resolution images of the ocean surface.
- **SAR Imagery Type**: 
  - The data used is typically **Level-1 Ground Range Detected (GRD)** or **Single Look Complex (SLC)** images, which provide detailed backscatter information for surface analysis.
- **Ingestion Mechanism**:
  - SAR data is periodically downloaded or requested through Sentinal 1 Data for Copernicus Browser, processed, and stored for further analysis.

---

## 2. **Preprocessing and Feature Extraction**

### a) **AIS Data Preprocessing**
1. **Noise Removal**:
   - Remove incomplete or erroneous AIS messages.
2. **Feature Engineering**:
   - Calculate derived features such as speed, acceleration, change in course, and distance between successive positions.
3. **Segmentation**:
   - Break AIS data into segments (e.g., based on time or spatial zones) for each vessel to facilitate analysis.

### b) **Satellite SAR Image Preprocessing**

---

## 3. **Modeling and Analysis**

### a) **Anomaly Detection in AIS Data**
Multiple machine learning models are used to detect irregularities in vessel behavior:

1. **DBSCAN (Density-Based Spatial Clustering of Applications with Noise)**:
   - **Purpose**: Identify anomalous vessel trajectories based on clustering.
   - **Method**: Group AIS data points based on location and time. Isolated points are flagged as anomalies, indicating sudden deviations or suspicious movements.

2. **Kalman Filter**:
   - **Purpose**: Predict the future state of the vessel (e.g., position, speed).
   - **Method**: Use the Kalman filter for tracking and predicting the expected trajectory of a ship. Deviations from predicted paths are treated as potential anomalies.

3. **Isolation Forest**:
   - **Purpose**: Detect outliers in multi-dimensional AIS data.
   - **Method**: The algorithm isolates anomalous behavior based on features like speed, course change, and acceleration.

4. **Autoencoder**:
   - **Purpose**: Detect complex anomalies by learning the normal behavior of vessels.
   - **Method**: An autoencoder neural network is trained on normal AIS data patterns. During inference, any significant reconstruction error indicates an anomaly.

### b) **Oil Spill Detection using SAR Satellite Images**

1. **Image Classification with ResNet50**:
   - **Purpose**: Classify SAR image patches as either "Oil Spill" or "No Oil Spill".
   - **Method**: A pretrained **ResNet50** model is fine-tuned on SAR image datasets labeled with oil spill and non-oil spill regions. It extracts deep features from the images and outputs a classification score.

2. **Image Segmentation with UNet**:
   - **Purpose**: Segment the specific areas of an oil spill within a given SAR image.
   - **Method**: The UNet architecture is used to create pixel-wise segmentation masks for the oil spill areas. It is particularly effective for detecting small, irregularly shaped oil patches and provides spatial boundaries of the spill.

3. **Object Detection with YOLOv5**:
   - **Purpose**: Detect and localize oil spill regions and ships simultaneously.
   - **Method**: YOLOv5 (You Only Look Once) is trained on annotated SAR images to detect both ships and oil spill regions in a single forward pass. The bounding box coordinates are used to identify the exact locations of spills and nearby ships.

---

## 4. **Integration of AIS and Satellite Data**

1. **Correlation Analysis**:
   - Once an anomaly is detected in AIS data, the corresponding satellite data is retrieved for the same timestamp and location.
   - The system cross-references the AIS-detected anomaly location with the satellite data to confirm oil spills or other irregularities.

2. **Enhanced Detection**:
   - Satellite data is used to validate AIS anomalies. For example:
     - If a vessel shows erratic movement in AIS data, and the same area in the satellite image shows an oil spill, it strongly indicates the vessel is the source.
     - If no anomaly is detected in AIS data, but an oil spill is detected in the satellite image, further investigation is required.

---

## 5. **Reporting and Dashboard Visualization**

1. **Real-Time Monitoring**:
   - A live AIS map is provided on the dashboard, showing vessel positions, detected anomalies, and potential spill areas.
   - Users can zoom into specific vessels, view historical trajectories, and investigate flagged anomalies.

2. **Alert Generation**:
   - Whenever a potential anomaly or oil spill is detected, the system sends real-time alerts to the dashboard, along with visual cues.

3. **Detailed Reports**:
   - For each detected spill or anomaly, a comprehensive report is generated.
   - **Contents**:
     - AIS anomaly summary (e.g., vessel ID, time of detection, type of anomaly).
     - Satellite-based oil spill detection (location, size, spill extent).
     - Correlation results, if both AIS and satellite data are used.
   
4. **Database Storage**:
   - All detected events and generated reports are stored in the system database for historical analysis and compliance purposes.

---

## 6. **System Components and Interactions**

### a) **Dashboard Interface**:
- A React.js-based frontend that provides an interactive interface for monitoring vessel movements and viewing real-time data.

### b) **Backend Services**:
- **FastAPI** is used to serve the anomaly detection API, allowing the integration of multiple detection models (DBSCAN, Kalman, Isolation Forest, etc.).
- A separate microservice handles oil spill detection from satellite data and generates reports.

### c) **Data Pipelines**:
- **Kafka Stream**: Manages real-time AIS data flow.
- **Database Service**: PostgreSQL or MongoDB for storing AIS logs, detected anomalies, and report data.

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
