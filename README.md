# QoS_Prediction_Model

## Overview

**What this pipeline does:**
1. **Capture Network Traffic** using `tshark` and `pyshark`.
2. **Extract Packet-Level Features** from PCAP files.
3. **Collect QoS Metrics** (e.g., download/upload speeds) with `speedtest-cli`.
4. **Merge and Preprocess Data** to create a training-ready dataset.
5. **Train a Neural Network Model** (using PyTorch) to predict QoS metrics.
6. **Explain the Model** using a decision-tree-based approach (`trustee`)

## Different Data Options

Can select different working nodes to get different data sets

