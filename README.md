# Valve Condition Prediction Project

This repository contains a machine learning pipeline designed to predict the condition of a hydraulic valve based on sensor data from a hydraulic test rig. 
The project involves building a classification model to determine if the valve condition is optimal (=100%) or not for each cycle. 
The dataset consists of sensor readings (pressure and volume flow) and associated condition labels for each cycle. 
The model is trained using the first 2000 cycles, and the remaining data serves as the final test set.

The raw sensor data includes pressure readings sampled at 100Hz and volume flow readings at 10Hz. The target condition (valve condition) is one of several possible labels, including optimal, small lag, severe lag, or failure. The pipeline processes this data, applies feature extraction, and trains a classification model using a Random Forest Classifier. 
Model performance is evaluated based on accuracy (since the target data is balanced) and the model's ability to generalize to unseen data is a key focus of this work.

We extracted various time-based features from the sensor data of each cycle, focusing on descriptive statistics such as mean, max, min, standard deviation, and percentiles for both pressure (PS2) and volume flow (FS1). Additionally, we calculated the number of peaks and troughs in both FS1 and PS2 to capture fluctuations and trends over time. These features help represent the dynamic behavior of the system during the cycles. We chose to focus on these time-based features, but other features, such as frequency domain features, could have been added for further enhancement.Based on these features, we attained 91% of accuracy on unseen data.


## Repository Structure
- `.github/workflows/`: GitHub actions workflows for CI/CD and MLOps
- `experimental/`: Contains script to test the prediction flask app
- `src/`: Contains Python scripts for data processing, training, and monitoring.
- `tests/`: Contains scripts for unit testing.
- `Dockerfile`: Docker file for deploying the model API.
- `configuration.ini`: Configuration for model parameters and file paths.
- `requirements.txt`: Requirements file.

## Setup and Installation

### Install Dependencies
Clone the repository and install dependencies:
```bash
git clone https://github.com/mahmoudalbardan/test_vif.git
cd test_vif
pip install -r requirements.txt
```

##  Launch and test the app on your Local Machine
To train the model locally, use the data already stored in Google Cloud Storage
by following these steps:

1. Run the training script:
```bash
python src/scripts/train_model.py --configuration configuration.ini --retrain false
```
The model will be automaticaly saved in: `src/model/fraud_detection_model.pkl`

2. Build docker image:
```bash
docker build -t fraudapp .
```
3.  Run docker container:
```bash
docker run -p 5000:5000 fraudapp
```
4. Test the app by running the following script:
```bash
python experimental/testing_the_app.py
```
