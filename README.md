# QoS_Prediction_Model

## Overview

This project demonstrates how to predict Quality of Service (QoS) metrics (e.g., download/upload speeds, latency) from the initial packets captured during a speed test. Instead of running prolonged tests that consume time and resources, we leverage early packet features to make accurate predictions. By integrating data collection, feature extraction, model training, and explainability, we create a reproducible pipeline to showcase how to optimize speed testing.

**What this pipeline does:**
1. **Capture Network Traffic**: Use `tshark` and `pyshark` to capture and parse packet-level data.
2. **Extract Packet-Level Features**: Focus on the earliest packets in a speed test and extract features like packet size, inter-arrival times, TCP headers, etc.
3. **Collect QoS Metrics**: Run a speed test command (`speedtest-cli` or `speedtest`) to measure final QoS metrics.
4. **Merge and Preprocess Data**: Combine packet features with QoS metrics into a single dataset, handling missing values and encoding categorical variables.
5. **Train a Neural Network Model (PyTorch)**: Train a feed-forward neural network to predict QoS metrics from initial packet features.
6. **Explain the Model (Trustee)**: Use Trustee to approximate the neural network’s behavior with a decision tree, providing a global interpretation of which features matter most.

## Single Notebook Structure

All code for data collection, preprocessing, training, and explanation is contained in a single Jupyter notebook: `data_collection.ipynb`. This notebook:
- Runs the pipeline multiple times (optional), if desired, to collect data.
- Preprocesses the merged dataset (features and QoS metrics).
- Trains the neural network model.
- Uses Trustee to explain the trained model.

## Setup Instructions

### Prerequisites
- **Python 3.8+**
- **Jupyter Notebook**: To run `data_collection.ipynb`.
- **Network Tools**: `tshark`, `curl`
- **Python Packages**:
  - `pyshark` for packet parsing
  - `speedtest-cli` or official `speedtest` binary for QoS metrics
  - `pandas`, `numpy`, `scikit-learn` for data handling and preprocessing
  - `torch` for the neural network
  - `trustee` for model explanation
  - `matplotlib` or `plotly` if you want additional visualizations

Install packages via:
```
pip install pyshark speedtest-cli scikit-learn torch torchvision trustee pandas numpy matplotlib
```

Make sure `tshark` and `curl` are installed:
```
sudo apt-get update && sudo apt-get install -y tshark curl
```

If using Ookla’s official speedtest:
```
curl -s https://install.speedtest.net/app/cli/install.deb.sh | sudo bash
sudo apt-get install speedtest
```

## Running the Pipeline

1. **Open the Notebook**:
   Start Jupyter:
   ```
   jupyter notebook
   ```
   
   Then open `data_collection.ipynb` in your browser.

2. **Configure Environment Variables (If Using NetUnicorn)**:
   If you have NetUnicorn environment variables like `NETUNICORN_ENDPOINT`, `NETUNICORN_LOGIN`, and `NETUNICORN_PASSWORD`, set them before running cells in the notebook. The code will use these for remote execution if needed.

3. **Data Collection (Optional)**:
   If the code in `data_collection.ipynb` includes sections to run the pipeline multiple times, execute those cells to:
   - Install dependencies
   - Start packet capture
   - Run speed tests
   - Stop capture and extract features
   - Collect QoS metrics
   - Save `features_<unique_id>.json` and `qos_metrics_<unique_id>.json`

   Adjust parameters like `num_runs` and node selection within the notebook cells.

4. **Preprocessing**:
   The notebook contains cells to merge the collected JSON files into a single dataset. It will:
   - Load `features.json` and `qos_metrics.json` from previous runs
   - Encode categorical features (e.g., `protocol`)
   - Scale numerical features
   - Split into training and testing sets

   Simply run these cells in order.

5. **Model Training**:
   Next, run the training cells:
   - Define the PyTorch model (e.g., `QoSPredictor`)
   - Train using the provided dataset, printing training and test losses
   - Adjust hyperparameters (hidden units, learning rate, epochs) within the notebook if desired.

6. **Model Explanation (Trustee)**:
   Finally, run the cells that use Trustee:
   - Wrap the trained model with a `predict` method.
   - Use `RegressionTrustee` to approximate the neural network’s predictions with a decision tree.
   - Display fidelity scores and optionally visualize the tree.

   This step shows you which features are most influential in predicting QoS.

## Reproducing Results from the Report

To reproduce the results:
1. **Collect Sufficient Data**: Run the data collection cells multiple times to gather enough samples. Adjust conditions (like waiting between runs) as indicated in the report.
2. **Preprocess and Scale Data**: Execute preprocessing cells exactly as described in the notebook.
3. **Train the Model**: Run the training cell multiple epochs (e.g., 1000 epochs) or tweak hyperparameters as the report states.
4. **Check Loss and Metrics**: Ensure training/test losses match the approximate numbers in the report.
5. **Run Trustee Explanation**: Compare the produced decision tree and fidelity scores to the ones mentioned in the report. If they differ, ensure you used the same dataset size, model parameters, and conditions.

## Tips and Troubleshooting

- If `speedtest-cli` fails, consider retry logic or use the official `speedtest`.
- If you get NaNs in training, check preprocessing steps and ensure no columns are all NaN or zero variance.
- If `tshark` requires root access, run Jupyter with appropriate permissions or run capture steps in a separate environment.
- Refer to code comments in `data_collection.ipynb` for detailed explanations of each step.

## Contact

For questions or issues, please raise a GitHub Issue in this repository or contact the project authors.
