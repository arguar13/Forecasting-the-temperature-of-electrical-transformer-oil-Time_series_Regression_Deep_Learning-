*Read this in other languages: [Español](README_es.md)*
# Deep Learning Project — Multivariate Time Series Forecasting for Electrical Transformer Oil Temperature

This project develops a comprehensive deep learning benchmark for multivariate time series forecasting using the Electricity Transformer Dataset (ETDataset).

The objective is to predict the future Oil Temperature (OT) of electrical transformers based on historical temperature measurements, electrical load variables, and temporal features. Accurate forecasting of transformer oil temperature is critical for preventing overheating, improving asset reliability, and supporting predictive maintenance strategies in power distribution systems.

---

## About the Dataset

**Domain:** Energy Analytics & Industrial Monitoring
**Target Variable:** `OT` (Oil Temperature)

The project uses the ETTh1 dataset from the ETDataset collection, originally introduced in the research paper:

The dataset contains approximately two years of hourly measurements collected from electrical transformer stations.

### Available Variables

* HUFL — High Useful Load
* HULL — High Useless Load
* MUFL — Middle Useful Load
* MULL — Middle Useless Load
* LUFL — Low Useful Load
* LULL — Low Useless Load
* OT — Oil Temperature (Target)

Additional temporal features are extracted from timestamps:

* Month
* Day
* Hour

---

## Project Objective

Build and compare modern deep learning architectures capable of:

* Learning short-term and long-term temporal dependencies
* Forecasting transformer oil temperature from multivariate sequences
* Evaluating the effectiveness of attention-based and decomposition-based models
* Measuring both predictive performance and computational efficiency

---

## Forecasting Strategy

The problem is formulated as a supervised multivariate time series forecasting task.

### Sliding Window Approach

Historical observations are transformed into fixed-length sequences using rolling windows.

* Input Window: 48 hours
* Forecast Horizon: 1 step ahead

This allows neural networks to learn temporal patterns directly from sequential transformer behavior.

---

## Deep Learning Architectures

Five modern architectures are implemented and benchmarked.

### LSTM-Attention
### Vanilla Transformer
### PatchTST
### DLinear
### ConvTransformer

---

## Deep Learning Pipeline

The project includes a complete forecasting workflow:

* Data ingestion and preprocessing
* Exploratory data analysis (EDA)
* Temporal feature engineering
* Data normalization
* Sliding window sequence generation
* PyTorch DataLoader creation
* Model implementation
* Hyperparameter optimization with Optuna
* Training with Early Stopping
* Benchmark evaluation
* Forecast visualization

---

## Hyperparameter Optimization

### Optuna

Bayesian optimization is used to identify the optimal learning rate for training.

**Optimized Parameter:**

* Learning Rate

The best configuration is then applied across the benchmark experiments.

---

## Evaluation Metrics

Model performance is evaluated using:

* MAE (Mean Absolute Error)
* MSE (Mean Squared Error)
* R² Score
* Training Time

This enables comparison between forecasting accuracy and computational efficiency.

---

## Visualizations

The project generates several analytical visualizations:

### Exploratory Analysis

* Oil Temperature Time Series
* Correlation Heatmap

### Benchmark Analysis

* MAE Comparison
* MSE Comparison
* Execution Time Comparison

### Forecasting Analysis

* Actual vs Predicted Temperature Curves
* Forecasting Performance Visualization

---

## Industrial Applications

* Predictive maintenance
* Transformer health monitoring
* Power grid reliability
* SCADA decision support systems
* Load management
* Thermal risk detection
* Asset lifecycle optimization
* Energy infrastructure analytics

---

## License

Educational and research-oriented Deep Learning project.

---

## Author

**Armando Guarnera**
Data Scientist
Argentina