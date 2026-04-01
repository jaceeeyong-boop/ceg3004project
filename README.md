# CEG3004 Project – Environmental Sound Classification

Group:Pr_15

## 1. Project Overview
This project implements an Environmental Sound Classification system using Digital Signal Processing (DSP) feature extraction and machine learning classification. The objective is to classify audio clips into 50 sound classes and ensure the model performs well under clean, noisy, and band-limited conditions.

This project is based on the provided baseline code, and improvements were made in the following areas:
- Audio preprocessing
- DSP feature extraction
- Model selection

## 2. Aim
The aim is to design a robust audio classification pipeline for Environmental Sound Classification that performs well under clean and distorted conditions.


## 3. Objectives
The objectives of this project are:
- Train on labeled environmental sound data
- Extract meaningful DSP features
- Use a machine learning classifier to classify into the 50 sound classes
- Demonstrates robustness to noise and bandwidth distortions


## 4. Dataset Description
The dataset is derived from the ESC-50 dataset and contains:
- 2,000 audio clips
- 50 environmental sound classes
- 40 clips per class
- Each clip is 5 seconds long
- Mono waveform audio

The evaluation dataset contains three versions of each clip:
- Clean audio
- Noisy audio
- Band-limited audio

The model must perform well under all three conditions.

## 5. Methodology

### Experimental Approach and Methodology

To obtain a robust environmental sound classification system, a systematic experimental approach was used instead of changing all components at once. The goal was to identify which component (preprocessing, feature extraction, or model) contributed most to performance improvement.

The experiments were conducted in three stages:
1. Stage 1 – Preprocessing Selection
2. Stage 2 – Feature Extraction Selection
3. Stage 3 – Model Selection

In each stage, only one component was changed while the others were kept constant. This ensures a fair comparison and allows the impact of each component to be evaluated independently.

This experimental approach was chosen because environmental sound classification performance is affected by multiple factors, including signal quality, feature representation, and classifier type. By testing each component separately, the most robust combination of preprocessing, feature extraction, and model could be identified.

The evaluation metric used for all experiments was Macro-F1 score, as it measures classification performance across all classes equally and is suitable for multi-class classification problems such as ESC-50.

### 5.1 Audio Preprocessing
Different preprocessing techniques were tested to improve audio quality and consistency:
- Silence trimming
- Peak normalization
- RMS normalization
- Fixed-length padding/truncation (5 seconds)
- Pre-emphasis filtering

Experimental Setup (Stage 1)
| Parameter | Setting |
|-----------|--------|
| Feature Set | MFCC + Delta + Delta-Delta |
| Model | Support Vector Classifier (SVC) |
| Evaluation Metric | Macro-F1 score |

Only preprocessing was changed in Stage 1.

### Stage 1 Results – Preprocessing Experiments
| Exp | Preprocessing | Features | Model | Macro-F1 |
|-----|--------------|----------|-------|----------|
| P0 | Baseline | MFCC+d1+d2 | SVC | 0.4768 |
| P1 | Trim + Peak Norm + Fixed Length | MFCC+d1+d2 | SVC | **0.5268** |
| P2 | Trim + RMS Norm + Fixed Length | MFCC+d1+d2 | SVC | 0.5241 |
| P3 | P1 + Pre-emphasis | MFCC+d1+d2 | SVC | 0.5190 |

**Best preprocessing: Trim + Peak Normalization + Fixed Length**

### Discussion
From the results, it can be observed that applying preprocessing significantly improved the Macro-F1 score compared to the baseline without preprocessing. The baseline model achieved a Macro-F1 score of 0.4768, while all preprocessing methods achieved higher scores.

Among the tested methods, P1 (Trim + Peak Normalization + Fixed Length) achieved the highest Macro-F1 score of 0.5268, indicating that removing silence, normalizing amplitude, and standardizing clip length improves feature consistency and classification performance.

Although RMS normalization (P2) also performed well, its performance was slightly lower than peak normalization. The addition of pre-emphasis (P3) did not further improve performance, suggesting that boosting high-frequency components was not necessary for this dataset.

Therefore, P1 was selected as the final preprocessing pipeline for subsequent feature extraction experiments.

### 5.2 Feature Extraction (DSP Features)

The following DSP features were extracted:
- MFCC
- Delta MFCC
- Delta-Delta MFCC
- Log-Mel Spectrogram
- Spectral Centroid
- Spectral Bandwidth
- Spectral Rolloff
- Spectral Flux
- Robust Pooling (mean, std, median, percentiles)
- CMVN (Cepstral Mean and Variance Normalization)
- Multi-window features

### Experimental Setup (Stage 2)
| Parameter | Setting |
|-----------|--------|
| Preprocessing | Best preprocessing from Stage 1 |
| Model | Support Vector Classifier (SVC) |
| Evaluation Metric | Macro-F1 score |

Only feature extraction was changed in Stage 2.

### Stage 2 Results – Feature Extraction Experiments
| Exp | Preprocessing | Features | Model | Macro-F1 |
|-----|--------------|----------|-------|----------|
| F1 | Best preprocessing | MFCC+d1+d2 | SVC | 0.5268 |
| F2 | Best preprocessing | F1 + Robust Pooling + Log-Mel | SVC | 0.5891 |
| F3 | Best preprocessing | F2 + Spectral Features | SVC | **0.6054** |
| F4 | Best preprocessing | F3 + CMVN on Log-Mel | SVC | 0.5480 |
| F5 | Best preprocessing | F3 + Multi-window | SVC | 0.5760 |

**Best feature set: MFCC + Delta + Delta-Delta + Log-Mel + Spectral Features + Robust Pooling**

### Discussion 
From the Stage 2 results, it can be observed that feature extraction significantly improved the Macro-F1 score compared to using MFCC features alone.

The baseline feature set (MFCC + Delta + Delta-Delta) achieved a Macro-F1 score of 0.5268. When log-mel spectrogram and robust pooling were added (F2), the Macro-F1 score increased to 0.5891. This shows that log-mel spectrogram provides additional time-frequency information that MFCC alone may not capture.

When spectral features (spectral centroid, bandwidth, rolloff, and flux) were added in F3, the Macro-F1 score further improved to 0.6054, which was the best result in Stage 2. These spectral features describe the frequency distribution, brightness, and changes in the audio signal, which are useful for distinguishing different environmental sounds.

Applying CMVN in F4 reduced the performance slightly, suggesting that normalization on log-mel features did not improve classification performance for this dataset.

Multi-window features (F5) also improved performance compared to the baseline but did not outperform F3. Therefore, F3 was selected as the final feature set.

Overall, the results show that combining MFCC, log-mel spectrogram, spectral features, and robust pooling produces a richer representation of the audio signal and improves classification performance.

### 5.3 Models Evaluated
The following machine learning models were tested:

| Model | Type |
|------|------|
| Logistic Regression | Linear |
| Random Forest | Non-linear |
| Support Vector Classifier (SVC) | Non-linear |

### Experimental Setup (Stage 3)
| Parameter | Setting |
|-----------|--------|
| Preprocessing | Best preprocessing |
| Features | Best feature set |
| Evaluation Metric | Macro-F1 score |

### Stage 3 Results – Model Comparison
| Exp | Model | Macro-F1 |
|-----|------|----------|
| M1 | Support Vector Classifier (SVC) | **0.6054** |
| M2 | Logistic Regression | 0.4730 |
| M3 | Random Forest | 0.5356 |

**Final model selected: Support Vector Classifier (SVC)**

### Discussion
In Stage 3, different machine learning models were compared using the best preprocessing and best feature set obtained from Stage 1 and Stage 2.

Logistic Regression produced the lowest Macro-F1 score because it is a linear model and may not be able to capture complex non-linear relationships in environmental sound features.

Random Forest performed better than Logistic Regression because it is a non-linear model and can model more complex decision boundaries. However, its performance was still lower than SVC.

Support Vector Classifier (SVC) achieved the best performance among all models. This is because SVC with RBF kernel is effective in handling high-dimensional feature spaces and can model complex non-linear relationships between features and sound classes.

Therefore, SVC was selected as the final model for this project.

## 6. Final Pipeline
The final audio classification pipeline:

| Component | Method |
|----------|-------|
| Preprocessing | Trim + Peak Normalize + Fixed Length |
| Features | MFCC + Delta + Delta-Delta + Log-Mel + Spectral + Robust Pooling |
| Model | Support Vector Classifier (RBF Kernel) |
| Metric | Macro-F1 |


## 7. Repository Structure
.
├── README.md
├── ceg3004_project_colab.py
├── experiment_results.xlsx
├── TEAM_15_predictions.csv
├── TEAM_15_validation.csv

## 8. How to Run the Project

Step 1 – Open the notebook

Run the provided notebook:
```bash
ceg3004_project_colab.ipynb
```

Step 2 – Set TEAM_ID

Update the TEAM_ID variable at the top of the notebook.

Step 3 – Train the model

The code will:

Load audio files
Extract features
Train the model
Evaluate Macro-F1 score
Save the trained model

Step 4 – Generate predictions
The code will generate:

- Prediction file (TEAM_ID_predictions.csv)
- Validation results (TEAM_ID_validation.csv)

## 9. Reproducibility

To reproduce results:

Use the provided dataset
Set the correct TEAM_ID
Run all notebook cells in order
Use the final selected preprocessing and feature extraction pipeline
Train the SVC model and generate predictions

## 10. Team Contribution
| Team Member | Contribution |
|-------------|-------------|
| Yeoh Hao Wei | Audio preprocessing, DSP feature extraction, GitHub documentation |
| Jacelyn Yong | Model training, experiments, GitHub documentation |
| Both | Evaluation, testing, final submission |

## 11. Conclusion
This project demonstrates that combining meaningful DSP feature extraction with a non-linear classifier such as SVC can achieve strong performance in environmental sound classification. Experimental comparisons show that preprocessing, feature engineering, and model selection all significantly affect classification performance. The final system is robust, reproducible, and performs well under clean, noisy, and band-limited conditions.

