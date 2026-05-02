# Student Stress Detection from Wearable Sensor Data

Spring 2026 AI Class Project — Georgia State University

## Overview
This project predicts university students' daily stress levels using physiological and behavioral data collected from Fitbit Inspire 3 wearable devices. Five machine learning models are implemented and compared using Leave-One-Student-Out (LOSO) cross-validation to ensure evaluation on unseen individuals.

Monitoring student mental health is a growing concern in academic environments. Wearable devices offer a continuous, non-invasive way to capture physiological signals such as heart rate variability and sleep quality, which are known indicators of psychological stress. This project investigates whether machine learning models trained on daily Fitbit sensor summaries can reliably classify self-reported stress levels into three categories: Low, Medium, and High.

## Dataset
SSAQS Dataset — Garcia Ceja et al. (2026)  
35 undergraduate students, one academic semester (February–July 2025)  
Collected at two Mexican universities using Fitbit Inspire 3 devices  
https://zenodo.org/records/18706837

Download the dataset, extract it, and place the student folders in the same directory as the notebook.

## Sensor Features
| Feature | Description |
|---------|-------------|
| hrv_rmssd_mean | Mean heart rate variability (key stress indicator) |
| hrv_rmssd_std | Variability of HRV throughout the day |
| hrv_lf_mean | Low frequency HRV power |
| hrv_hf_mean | High frequency HRV power |
| spo2_mean | Mean blood oxygen saturation |
| spo2_min | Minimum blood oxygen saturation |
| steps_total | Total daily step count |
| active_ratio | Ratio of active minutes to total recorded minutes |
| active_minutes | Total physically active minutes per day |
| sleep_score | Overall sleep quality score |
| deep_sleep_min | Deep sleep duration in minutes |
| fitbit_stress | Fitbit built-in stress score |

## Models
| Model | Description |
|-------|-------------|
| Random Forest | Ensemble of decision trees, robust to noisy sensor data |
| LightGBM | Gradient boosting, effective for tabular physiological data |
| CNN | 1D convolutional network, detects local patterns across sensor features |
| LSTM | Recurrent network, captures temporal dependencies in daily readings |
| Fuzzy Logic | Rule-based inference using HRV, SpO2, and sleep quality |

## Methodology
Raw sensor files (HRV, SpO2, steps, activity, sleep, stress) are aggregated to daily summaries per student. Missing values are filled using each student's own mean, followed by the global median. Stress labels are assigned per student based on individual percentile thresholds (33rd and 67th percentile), resulting in three balanced classes: Low (0), Medium (1), and High (2).

All models are evaluated using Leave-One-Student-Out (LOSO) cross-validation: each student is held out as the test set once while the model trains on the remaining 34 students. This ensures the model is tested on individuals it has never seen during training.

## Results
| Model | Accuracy | F1-Score | MAE |
|-------|----------|----------|-----|
| Random Forest | 0.3500 | 0.3439 | 0.9224 |
| LightGBM | 0.3552 | 0.3532 | 0.8809 |
| CNN | 0.3596 | 0.2559 | 0.9828 |
| LSTM | 0.3587 | 0.2900 | 0.9729 |
| Fuzzy Logic | 0.3012 | 0.2059 | 0.7400 |

LightGBM achieved the highest accuracy and F1-score. Fuzzy Logic achieved the lowest MAE, meaning its predictions were closest to the true stress label on average. Physical activity features (active_ratio, steps_total) were identified as the most important predictors by the Random Forest feature importance analysis.

## Limitations and Future Work
- **Personalization:** LOSO validation reveals high individual variability in stress responses. Personalized models fine-tuned per student may improve performance significantly.
- **Temporal modeling:** LSTM currently treats each day independently. Using multi-day windows as input sequences would better leverage temporal patterns.
- **Label imbalance:** Stress labels are derived from self-reports, which may not fully capture physiological stress.
- **Dataset size:** 35 students limits generalizability. Larger datasets with diverse populations are needed.

## References
[1] E. Garcia Ceja, J. Alvarado-Uribe, P. J. Escamilla-Ambrosio, A. Lara, A. Mena-Martinez, G. Gallegos-Garcia, M. Gonzalez-Mendoza, R. Monroy, G. Martinez Luna, and J. M. Fernández-Cárdenas, "A Dataset of University Students' Stress and Anxiety Levels based on Questionnaires and Wearable Sensors," Zenodo, Feb. 20, 2026. doi: 10.5281/zenodo.18706837.
