*Read this in other languages: [Español](README_es.md)*
# Deep Learning Project — Multivariate Time Series Forecasting for Electrical Transformer Oil Temperature

This project develops a comprehensive deep learning benchmark for multivariate time series forecasting using the Electricity Transformer Dataset (ETDataset).

The objective is to predict the future Oil Temperature (OT) of electrical transformers based on historical temperature measurements, electrical load variables, and temporal features. Accurate forecasting of transformer oil temperature is critical for preventing overheating, improving asset reliability, and supporting predictive maintenance strategies in power distribution systems.

---

## About the Dataset

**Domain:** Energy Analytics & Industrial Monitoring
**Target Variable:** `OT` (Oil Temperature)

The project uses the ETTh1 dataset from the [ETDataset](https://github.com/zhouhaoyi/ETDataset) collection, originally introduced in the research paper:

> Zhou, H. et al. (2021). *Informer: Beyond Efficient Transformer for Long Sequence Time-Series Forecasting.* AAAI 2021.

A copy of the official repository is included in `data/ETDataset-main.zip`.

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
Bidirectional LSTM with an additive attention layer that weights the 48 hidden states before the regression head.

### Vanilla Transformer
Linear input projection + sinusoidal positional encoding + 2 Transformer encoder layers, mean-pooled over time.

### PatchTST
The 48-hour window is split into 4 patches of 12 hours; each patch is a token for a 2-layer Transformer encoder.

### DLinear
Decomposes the window into trend (moving average) and seasonal components and applies a linear layer to each one.

### ConvTransformer
1D convolution to extract local patterns, followed by a 2-layer Transformer encoder.

### Naive Baseline (Persistence)
`OT(t) = OT(t-1)`. For one-step-ahead forecasting of a highly autocorrelated series this is the reference any model has to beat.

---

## Deep Learning Pipeline

The project includes a complete forecasting workflow:

* Data ingestion and preprocessing
* Exploratory data analysis (EDA)
* Temporal feature engineering
* Chronological train / validation / test split (70% / 10% / 20%)
* Data normalization (scalers fitted on train only)
* Sliding window sequence generation
* PyTorch DataLoader creation
* Model implementation
* Hyperparameter optimization with Optuna
* Training with Early Stopping (best weights restored)
* Benchmark evaluation
* Forecast visualization

---

## Hyperparameter Optimization

### Optuna

Bayesian optimization (TPE sampler, 20 trials on the DLinear model) is used to identify the optimal learning rate for training.

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

## Results

Test set (last 20% of the series, 3,484 hours). Metrics in the original scale (°C). Trained on an NVIDIA GTX 1650.

| Model | MAE (°C) | MSE | R² | Training time (s) |
|---|---|---|---|---|
| **DLinear** | **0.436** | **0.399** | **0.966** | 6.3 |
| Naive (persistence) | 0.448 | 0.428 | 0.964 | - |
| Vanilla Transformer | 0.604 | 0.601 | 0.949 | 16.3 |
| LSTM-Attention | 0.649 | 0.733 | 0.938 | 9.0 |
| ConvTransformer | 0.747 | 0.946 | 0.920 | 10.5 |
| PatchTST | 0.753 | 0.918 | 0.923 | 13.2 |

**Key findings**

* DLinear is the most accurate and the fastest model.
* The persistence baseline is very strong for one-step-ahead forecasting: only DLinear beats it. A high R² alone does not prove that a model adds value.
* Next steps: per-model hyperparameter tuning, longer horizons (24h / 48h), cyclical time features and averaging over several seeds.

---

## Project Structure

```
├── data/
│   └── ETDataset-main.zip          # Official ETDataset repository (ETTh1 is read from here)
├── notebooks/
│   └── Forecasting Oil Temperature.ipynb
├── requirements.txt
├── README.md
└── README_es.md
```

---

## How to Run

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows  (Linux/Mac: source .venv/bin/activate)
# Optional, for NVIDIA GPU:
pip install torch --index-url https://download.pytorch.org/whl/cu126
pip install -r requirements.txt
jupyter notebook "notebooks/Forecasting Oil Temperature.ipynb"
```

The notebook uses the GPU automatically when available (CPU also works, but training is much slower).

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
