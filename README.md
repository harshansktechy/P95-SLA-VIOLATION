# P95 SLA Violation Prediction in Microservices

## Overview

This project focuses on predicting **P95 tail-latency SLA violations in microservice architectures** using machine learning.

Using the Alibaba Microservice Cluster Trace dataset, the project combines service call-graph information, infrastructure resource metrics, and microservice traffic characteristics to construct a graph-aware dataset.

The objective is to identify whether a microservice interaction is likely to experience an SLA violation in the next time window based on its historical latency patterns, resource utilization, and service-level characteristics.

## Tech Stack & Libraries

- **Programming Language:** Python
- **Data Processing:** Pandas, NumPy
- **Machine Learning:** Scikit-learn
- **Data Visualization:** Matplotlib, Seaborn

## Dataset

The project uses the **Alibaba Microservice Cluster Trace Dataset**, incorporating multiple sources of microservice telemetry:

- **CallGraph:** Service-to-service communication and request latency information.
- **NodeMetricsUpdate:** Physical node CPU and memory utilization.
- **MSMetricsUpdate:** Microservice-level CPU and memory utilization.
- **MCRRTUpdate:** Microservice traffic and RPC/database response-time metrics.

These data sources are aggregated into 60-second time windows and combined to construct a unified dataset.

## Data Processing & Feature Engineering

The dataset-generation pipeline consists of the following stages:

1. **Call Graph Processing:** Aggregates service-to-service interactions and calculates P95, mean and maximum latency, request counts, and requests per second.

2. **Resource Metric Aggregation:** Combines microservice and physical-node CPU and memory utilization metrics.

3. **Graph Topology Features:** Extracts service connectivity characteristics, including fan-in, fan-out, and downstream service load.

4. **Dataset Integration:** Combines call-graph, resource-utilization, and traffic information into a unified graph-aware dataset.

5. **Time-Series Feature Engineering:** Generates historical latency features such as previous-window P95 latency, latency change, and rolling P95 trends over three and five windows. It also derives CPU-pressure indicators and a request-to-CPU ratio.

## SLA Violation Prediction

The project implements a binary classification approach to predict SLA violations in the next time window.

- **Target Variable:** `violation_next_window`
- **Class 1:** Next-window P95 latency exceeds the dynamically calculated SLA threshold.
- **Class 0:** Next-window P95 latency does not exceed the threshold.

The SLA threshold is calculated using the 95th percentile of the aggregated P95 latency values.

The target is generated using the subsequent time window's P95 latency, making the task a future-window prediction problem rather than a classification of the current window.

## Machine Learning Models

The repository includes implementations and experiments using the following classification algorithms:

1. Random Forest
2. Gradient Boosting
3. K-Nearest Neighbors (KNN)
4. Logistic Regression
5. Multi-Layer Perceptron (MLP)
6. Support Vector Machine (SVM)
7. Decision Tree
8. Naive Bayes
9. AdaBoost

These models are explored for predicting potential P95 SLA violations using the engineered microservice features.

## Repository Structure

| File | Description |
|---|---|
| `ML_Dataset.py` | Processes raw trace data and generates the graph-aware dataset |
| `RandomForest.ipynb` | Random Forest model |
| `Gradient Boosting.ipynb` | Gradient Boosting model |
| `KNN.ipynb` | K-Nearest Neighbors model |
| `Logistic Regression.ipynb` | Logistic Regression model |
| `MLP (1).ipynb` | Multi-Layer Perceptron model |
| `SVM.ipynb` | Support Vector Machine model |
| `decision-tree.ipynb` | Decision Tree model |
| `naive-bayes-classifier.ipynb` | Naive Bayes model |
| `AdaBoost (2).ipynb` | AdaBoost model |

## Execution

1. Clone the repository:

   ```bash
   git clone https://github.com/harshansktechy/P95-SLA-VIOLATION.git
   ```

2. Install the required libraries:

   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```

3. Obtain and prepare the required Alibaba microservice trace CSV files.

4. Update the `BASE_PATH` variable in `ML_Dataset.py` to point to the directory containing the raw dataset files.

5. Run `ML_Dataset.py` to generate the processed graph-aware dataset.

6. Open the individual Jupyter notebooks to train and evaluate the classification models.

**Note:** The raw dataset files are not included in this repository and must be obtained separately.

## Project Objective

The project explores how historical latency patterns, microservice communication topology, traffic characteristics, and infrastructure resource metrics can be combined to predict upcoming P95 SLA violations in microservice environments.