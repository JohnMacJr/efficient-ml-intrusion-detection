# Efficient Machine Learning for Network Intrusion Detection

This repository contains an efficiency-aware machine learning project for network intrusion detection using the CICIDS2017 dataset. The goal is to evaluate whether compact machine learning models can maintain strong intrusion detection performance while reducing computational cost for practical deployment.

## Project Overview

Network intrusion detection systems are used to identify malicious traffic in computer networks. Many machine learning IDS studies focus mainly on accuracy, but real-world deployment also depends on system-level constraints such as model size, inference latency, throughput, memory usage, and training time.

This project compares multiple machine learning models for multiclass intrusion detection and evaluates the trade-off between classification performance and computational efficiency.

## Dataset

The project uses the CICIDS2017 dataset, which contains labeled network traffic flows representing benign activity and several attack types.

After preprocessing, the original attack labels were consolidated into the following classes:

- Normal
- Bot
- Brute Force
- DDoS
- DoS
- Port Scanning
- Web Attack

## Preprocessing

The preprocessing pipeline includes:

- Merging CICIDS2017 CSV files
- Handling missing and infinite values
- Removing invalid flow-duration rows
- Removing duplicate records
- Dropping zero-variance features
- Removing identical/redundant columns
- Downcasting numeric columns to reduce memory usage
- Consolidating attack labels into broader IDS categories

The cleaned dataset was then used for feature selection and model training.

## Feature Engineering

Feature reduction was used as the main efficiency method. Instead of training models on the full feature set, the project selects a compact group of important network-flow features.

The feature selection process includes:

- Correlation filtering
- Statistical feature ranking
- Model-based feature importance analysis
- Final reduced feature set selection

The final models were trained using 10 selected features.

## Models

The following models were evaluated:

- Decision Tree
- XGBoost
- LightGBM

The Decision Tree serves as a lightweight baseline, while XGBoost and LightGBM are optimized gradient-boosting models. Optuna was used for hyperparameter tuning on the boosted models.

## Evaluation Metrics

Models were evaluated using both classification and efficiency metrics.

Classification metrics:

- Accuracy
- Precision
- Recall
- Weighted F1-score
- Per-class F1-score

Efficiency metrics:

- Model size
- Single-sample inference latency
- 95th-percentile latency
- Batch inference time
- Throughput
- Peak inference memory
- Training time

## Benchmark Summary

| Model | Accuracy | Weighted F1 | Model Size | Single-Sample Latency | Throughput |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 0.9901 | 0.9896 | 27 KB | 0.76 ms | 12.7M samples/s |
| XGBoost | 0.9954 | 0.9956 | 1007 KB | 2.92 ms | 1.96M samples/s |
| LightGBM | 0.9958 | 0.9959 | 834 KB | 1.29 ms | 61K samples/s |

The results show that LightGBM achieved the highest weighted F1-score, while the Decision Tree was significantly smaller and faster. This highlights the central trade-off of the project: boosted models provide stronger classification performance, but simpler models may be more practical for constrained deployment environments.
