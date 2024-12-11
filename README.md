# QoS_Prediction_Model

This repository contains a pipeline for capturing network packets, extracting features from packet capture (PCAP) files, collecting QoS metrics, merging them into a single dataset, and preparing that dataset for predictive modeling. 

## Overview

**What this pipeline does:**
1. **Network Packet Capture:** Uses `tshark` and `pyshark` to capture network packets and extract features such as packet size, timestamps, and TCP-related fields.
2. **QoS Data Collection:** Runs `speedtest-cli` to measure download speed, upload speed, and latency.
3. **Feature Extraction & Merging:** Converts raw packet data into JSON feature files and merges them with QoS metrics.
4. **Preprocessing:** Cleans and encodes the combined dataset, splits it into training/testing sets, and scales features.
5. **Model Preparation:** Produces a dataset ready for training predictive models that forecast network Quality of Service.


## Dependencies
- **Python Packages:**  
  - `pyshark` (Python wrapper for tshark)  
  - `speedtest-cli` (to run internet speed tests)  
  - `pandas` (for data manipulation)  
  - `requests` (for HTTP requests to fetch data)  
  - `scikit-learn` (for data preprocessing like scaling and splitting)

## Different Data Options

-**Can select different working nodes to get different data sets**
