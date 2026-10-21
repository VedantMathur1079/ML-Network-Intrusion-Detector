# ML Network Intrusion Detector

**Author:** Vedant Mathur

**Program:** IBM SkillsBuild Cybersecurity Bootcamp 2026 (BharatCares)

## Project Overview
A machine learning project that acts like a **security guard for a network**. For every network connection
(duration, protocol, bytes sent, login failures, etc.) the model decides whether it is **normal** or an **attack**.
Three models are trained and compared, and the best one is explained using feature importance.

## Problem Statement
Networks see thousands of connections every minute and humans cannot inspect them all. Automated detection of
malicious traffic (DoS floods, port scans, password guessing, privilege escalation) is a core part of cyber defence.

## Dataset
- **NSL-KDD** - a public benchmark dataset for network intrusion detection (improved version of KDD'99).
- Original source: University of New Brunswick, Canadian Institute for Cybersecurity -
  https://www.unb.ca/cic/datasets/nsl.html
- Files used (downloaded automatically by the notebook from this mirror):
  https://github.com/defcom17/NSL_KDD
  - `KDDTrain+.txt` - 125,973 connections (for training)
  - `KDDTest+.txt` - 22,544 connections (for testing, includes attack types not seen in training)
- Each connection has 41 features and a label (`normal` or an attack name). Attacks fall into 4 families:
  DoS, Probe, R2L (remote-to-local) and U2R (user-to-root).

## Technologies Used
- Python 3
- pandas, NumPy - data handling
- scikit-learn - preprocessing and models (Logistic Regression, Random Forest, Isolation Forest)
- matplotlib, seaborn - graphs
- Jupyter Notebook / Google Colab

## Methodology
1. Load the training and test data
2. Explore the data and group attacks into 4 families
3. Prepare the data: one-hot encode text columns, scale number columns
4. Train 3 models: Logistic Regression (baseline), Random Forest, Isolation Forest (anomaly detection)
5. Evaluate with accuracy, precision, recall, F1-score and confusion matrices
6. Check detection per attack family and explain predictions with feature importance

## Results (on the unseen test file)
| Model | Accuracy | Precision | Recall | F1-score |
|---|---|---|---|---|
| Logistic Regression | 0.7537 | 0.9174 | 0.6234 | 0.7424 |
| Random Forest | 0.7648 | 0.9670 | 0.6075 | 0.7462 |
| Isolation Forest | 0.7884 | 0.9365 | 0.6740 | 0.7839 |

Key findings:
- Random Forest reaches **99.9% accuracy** on a random split of the training data, but about **76%** on the official test file,
  because the test file contains new attack types (the "zero-day" problem).
- DoS (~79%) and Probe (~73%) attacks are detected well; R2L (~5%) and U2R (~12%) are rare and very hard to detect.
- Most important features: `src_bytes`, `dst_bytes`, `flag_SF`, `dst_host_same_srv_rate`, `dst_host_srv_count`.

## How to Run

### Option 1: Google Colab (easiest, nothing to install)
1. Go to https://colab.research.google.com and choose **File -> Upload notebook**.
2. Upload `Vedant_Mathur_ML_Network_Intrusion_Detector.ipynb`.
3. Click **Runtime -> Run all**. The dataset is downloaded automatically (internet required).

### Option 2: On your own computer
```bash
# 1. Install the libraries
pip install -r requirements.txt

# 2. Start Jupyter and open the notebook
jupyter notebook Vedant_Mathur_ML_Network_Intrusion_Detector.ipynb
```
Then run all cells from top to bottom. Graphs are saved in a `figures/` folder.

## Project Files
| File | Purpose |
|---|---|
| `Vedant_Mathur_ML_Network_Intrusion_Detector.ipynb` | Complete code with explanations and outputs |
| `requirements.txt` | Python libraries needed |
| `Vedant_Mathur_ProjectReport.docx` | Full project report |
| `README.md` | This file |

## Limitations and Future Work
- NSL-KDD is an older dataset; modern traffic differs.
- Rare attack classes (R2L, U2R) need special handling (e.g. SMOTE or class weights).
- Future work: use CIC-IDS2017, tune models, combine supervised and anomaly detection, build a live dashboard.
