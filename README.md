**Kinematic Data Analysis Pipeline for Patients with Lumbar Spinal Stenosis**

This repository contains the full implementation of an open-source, end-to-end pipeline for processing inertial measurement unit (IMU) data into clinically interpretable gait kinematics and biomechanical features, developed as part of a Master’s thesis in Biomedical Engineering.
The project focuses on objective gait assessment in patients with Lumbar Spinal Stenosis (LSS), addressing the limitations of proprietary gait analysis systems by preserving access to full time-series data, enabling transparency, reproducibility, and advanced biomechanical analysis.

**Project Goals**

The main objectives of this project are:\
-Transform raw IMU signals (accelerometer, gyroscope, magnetometer) into reliable segment orientations\
-Estimate joint kinematics using biomechanical modeling (OpenSim compatibility)\
-Validate IMU-derived joint angles against an optical motion capture gold standard (Qualisys)\
-Extract gait features and biomarkers capable of discriminating healthy subjects from LSS patients\
-Provide a fully automated, modular, and open pipeline suitable for research and clinical extension

**Why This Matters**

Most commercial IMU-based gait analysis tools:\
Are closed-source\
Output only summary tables\
Prevent access to full kinematic time-series\
Limit explainability and advanced analysis (e.g., feature engineering, machine learning)\
This pipeline was designed to:\
Be transparent and reproducible\
Preserve raw and processed signals\
Enable custom biomechanical and statistical analyses\
Support future decision-support systems for conservative vs. surgical treatment

**Data Overview**

Sensors: 15 Xsens MTw Awinda IMUs\
Signals per sensor:\
Accelerometer (AccX, AccY, AccZ)\
Gyroscope (GyrX, GyrY, GyrZ)\
Magnetometer (MagX, MagY, MagZ)\
Orientation (Roll, Pitch, Yaw – used only for validation)\
Sampling frequency: 40 Hz\
Each sensor stored in a separate file, mapped to a body segment\

**Pipeline Overview**

The project follows a strict sequential workflow, where each stage depends on the previous one:
Raw IMU Data\
   ↓\
Signal Preprocessing\
   ↓\
Sensor Fusion (Quaternion Orientation Estimation)\
   ↓\
Biomechanical Modeling (OpenSim)\
   ↓
Joint Angle Validation\
   ↓\
Gait Feature Extraction\
   ↓\
Statistical Group Analysis

**Repository Structure & File Order**

**Data Loading & Sensor Mapping**

Purpose:\
Load raw IMU data files and associate each sensor with its corresponding anatomical segment.\
Key operations:\
File parsing and synchronization\
Sensor-to-segment mapping\
Trial truncation and alignment\
Typical files:\
load_data.py\
sensor_mapping.py

**Signal Preprocessing**

Purpose:\
Prepare raw IMU signals for sensor fusion.\
Applied steps:\
Gyroscope bias correction and drift mitigation\
Accelerometer filtering (denoising, smoothing)\
Magnetometer hard-iron and soft-iron calibration\
Band-pass filtering and Savitzky–Golay smoothing\
Typical files:\
preprocess_gyroscope.py\
preprocess_accelerometer.py\
preprocess_magnetometer.py\
Output: Cleaned IMU signals per sensor

**Sensor Fusion (Orientation Estimation)**

Purpose:\
Estimate sensor orientation without relying on measured orientation for correction.\
Methods explored:\
Madgwick filter (primary)\
Mahony filter\
Complementary filtering (baseline)\
Key characteristics:\
Quaternion-based orientation estimation\
Euler angles derived only for interpretation\
Designed to avoid amplitude scaling using measured orientation\
Typical files:\
madgwick_filter.py\
sensor_fusion_pipeline.py\
Output:\
final_orientation_estimation_<sensor>.csv

**Biomechanical Modeling (OpenSim)**

Purpose:\
Convert segment orientations into anatomically meaningful joint angles.\
Steps:\
Quaternion-to-segment mapping\
Sensor-to-body alignment\
Export to OpenSim-compatible .sto files\
Use of a full-body musculoskeletal model\
Typical files:\
generate_opensim_sto.py\
opensim_pipeline.py

**Joint Angle Validation**

Purpose:\
Evaluate the accuracy of IMU-derived kinematics against optical motion capture.\
Metrics used:\
RMSE\
Total Angular Misalignment (TAM)\
Dynamic Time Warping (DTW)\
Intra-Phase Stability (IPS)\
Continuous Relative Phase (CRP)\
Typical files:\
validation_metrics.py\
compare_with_qtm.py

**Gait Feature Extraction**

Purpose:\
Extract biomechanical features from validated joint-angle time series.\
Feature domains:\
Range of motion\
Peak angles and timing\
Variability (SD, IQR, MAD)\
Temporal gait events\
Frequency-domain features\
Waveform similarity metrics\
Typical files:\
feature_extraction.py

**Statistical Analysis & Group Differentiation**

Purpose:\
Identify gait features associated with pathology severity.\
Analyses performed:\
Group comparisons (Healthy vs LSS)\
Conservative vs Surgical subgroup analysis\
Effect sizes and non-parametric statistics\
Typical files:\
statistical_analysis.py\
group_comparison.py

**Outputs**

Cleaned IMU signals\
Estimated segment orientations (quaternions & Euler angles)\
OpenSim-compatible joint kinematics\
Validation metrics vs motion capture\
Gait biomarkers and statistical summaries\
All intermediate results are saved explicitly to ensure traceability and reproducibility.

**Future Extensions**

This pipeline is designed to support:\
Unsupervised learning for gait pattern discovery\
Supervised classification for treatment stratification\
Longitudinal monitoring of disease progression\
Clinical decision-support tools
