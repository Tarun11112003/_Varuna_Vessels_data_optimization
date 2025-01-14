# Vessel Performance Analysis and Prediction

This project analyzes vessel performance data using various data analysis techniques, including data cleaning, feature engineering, visualization, linear regression, and neural networks. This file "vesselPerformance-Sample.json" contains the raw data, try.ipyb is the python file for all the processing done and final_processed_data contains the cleaned data.

## Table of Contents

* [Introduction](#introduction)
* [Data](#data)
* [Data Cleaning and Preprocessing](#data-cleaning-and-preprocessing)
* [Feature Engineering](#feature-engineering)
* [Data Visualization and Analysis](#data-visualization-and-analysis)
* [Model Choices and Hyperparameter Tuning](#model-choices-and-hyperparameter-tuning)
* [Key Performance Metrics and Interpretation](#key-performance-metrics-and-interpretation)
* [Deployment Instructions](#deployment-instructions)
* [Repository Structure](#repository-structure)


## Introduction

This project aims to derive insights from vessel performance data and build predictive models to estimate critical parameters like `SailedDistanceGPS` and `Engine.PowerAtShaft`. The analysis focuses on understanding the relationships between fuel consumption, engine parameters, and vessel speed, leveraging techniques like linear regression and neural networks.

## Data

The project uses the following datasets (replace placeholders with actual file names):

* `flattened_vesselPerformance copy.csv`: Primary dataset containing various vessel parameters.
* `Truefalse.csv`: Dataset with boolean-like values needing encoding.
* `exp.csv`: Dataset used for specific data cleaning tasks.
* `Engine_power_runn.csv`: Dataset focused on engine power and running hours.
* `fuel_robs_expanded.csv`: Dataset containing detailed fuel rob information.

## Data Cleaning and Preprocessing

1. **Handling Missing Values**:
   - Columns with >70% missing values were dropped.
   - Missing values in `Engine.PowerAtShaft`, `Engine.RPM`, and other key features were handled using median imputation (for skewed data) or forward-fill.
2. **Data Parsing**:
   - The `Consumptions` and `FuelRobs` columns (JSON-like strings) were parsed using `ast.literal_eval`, exploded, and normalized.
3. **True/False Encoding**:
   - Boolean-like columns were converted to 1/0, with nulls replaced by 0.
4. **Data Transformation**:
   - `Engine.RunningHours` was converted to minutes (`EngineRunningMinutes`).

## Feature Engineering

1. **Fuel_Consumption_per_NM**: Calculated by dividing `Consumptions_Parsed` by `SailedDistanceGPS` to normalize fuel efficiency. Infinite values were handled and replaced with the median.
2. **PCA_AirPressure and PCA_AirTemperature**: Principal Component Analysis (PCA) was applied to air pressure and temperature data to reduce dimensionality and capture the main variance in those related features.


## Data Visualization and Analysis

* **Line Plots and Histograms**: Visualized trends and distributions of `AirPressure` and `AirTemperature`.
* **Box Plots**: Examined variability and outliers in air pressure and temperature.
* **Fuel Consumption Analysis**: Explored fuel consumption patterns based on engine type and fuel type using bar plots and KDE plots.  Key findings include main engine dominance, VLSFO preference, and low auxiliary consumption.  Specific optimization suggestions for HFO, MGOLS, and VLSFO usage are detailed in the Jupyter Notebook. 

## Model Choices and Hyperparameter Tuning

1. **Linear Regression:**  Used to predict `SailedDistanceGPS` from `Consumptions_Parsed` due to observed linear relationship.  Achieved reasonable performance (MSE: 1775.19, R²: 0.7444).
2. **Neural Network:** Attempted to predict `Engine.PowerAtShaft` from `Consumptions_Parsed`.  Despite tuning (2 hidden layers, Adam optimizer, 100 epochs), the model performed poorly (negative R²).

## Key Performance Metrics and Interpretation

* **Linear Regression**: MSE: 1775.19, R²: 0.7444.  The model adequately captured the linear relationship, allowing for imputation of missing `SailedDistanceGPS` values.
* **Neural Network**: RMSE: 662.45, R²: -1.0739. The model did not effectively capture underlying patterns.

## Deployment Instructions

1. **Using Docker:**
    * Build the image: `docker build -t vessel-performance .`
    * Run the container, mounting your data directory: `docker run -v /path/to/your/data:/app/data vessel-performance`
2. **Running Locally (Without Docker):**
    * Create a virtual environment (recommended).
    * Install required packages using `pip install -r requirements.txt`.
    * Run the `try.ipynb` Jupyter Notebook or the equivalent python script.

## Repository Structure

* `try.ipynb` : Jupyter Notebook containing the core analysis and modeling code.
* `requirements.txt`: Lists the required Python packages.
* `Dockerfile`: Instructions for building the Docker image.
* `README.md`: This documentation file.

