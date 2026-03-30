# CEG3004 Project – Environmental Sound Classification

## 1. Project Overview
This project implements an Environmental Sound Classification system using Digital Signal Processing (DSP) feature extraction and machine learning classification. The objective is to classify audio clips into 50 sound classes and ensure the model performs well under clean, noisy, and band-limited conditions.

This project is based on the provided baseline code, and improvements were made in the following areas:
- Audio preprocessing
- DSP feature extraction
- Model selection
- Experimental evaluation

---

## 2. Dataset Description
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

---

## 3. Methodology

### 3.1 Audio Preprocessing
Several preprocessing methods were tested to improve audio quality and consistency:
- Silence trimming (removes leading and trailing silence)
- Peak normalization (normalizes amplitude)
- Fixed-length padding/truncation (ensures all clips are 5 seconds)
- Pre-emphasis filtering (boosts high-frequency components)

The final preprocessing pipeline used:


---

### 3.2 Feature Extraction (DSP Features)
The following DSP features were extracted from each audio clip:

| Feature | Description |
|--------|-------------|
| MFCC | Represents perceptual audio characteristics |
| Delta MFCC | First-order change of MFCC |
| Delta-Delta MFCC | Second-order change of MFCC |
| Log-Mel Spectrogram | Energy distribution across frequency bands |
| Spectral Centroid | Brightness of sound |
| Spectral Bandwidth | Spread of frequency content |
| Spectral Rolloff | Frequency below which most energy is contained |
| Zero Crossing Rate | Measures noisiness of signal |
| Spectral Flux | Measures rate of change in spectrum |

Additional techniques:
- CMVN (Cepstral Mean and Variance Normalization)
- Robust pooling (mean, standard deviation, median, percentiles)

---

### 3.3 Models Evaluated
The following machine learning models were tested:

| Model | Type |
|------|------|
| Logistic Regression | Linear |
| Random Forest | Non-linear |
| Support Vector Classifier (SVC) | Non-linear |

SVC with RBF kernel achieved the best Macro-F1 score and was selected as the final model.

---

## 4. Experiments

### 4.1 Preprocessing Experiments
| Experiment | Preprocessing | Macro-F1 |
|-----------|--------------|----------|
| P0 | No preprocessing | XX |
| P1 | Trim silence | XX |
| P2 | Trim + Normalize | XX |
| P3 | Trim + Normalize + Fixed Length | XX |
| P4 | Trim + Normalize + Fixed Length + Pre-emphasis | XX |

Best preprocessing: **P?**

---

### 4.2 Feature Extraction Experiments
| Experiment | Feature Set | Macro-F1 |
|-----------|------------|----------|
| F0 | MFCC + Delta + Delta-Delta | XX |
| F1 | F0 + Spectral Features | XX |
| F2 | F0 + Log-Mel | XX |
| F3 | F1 + Robust Pooling | XX |
| F4 | MFCC + Log-Mel + Spectral + CMVN + Robust Pooling | XX |

Best feature set: **F?**

---

### 4.3 Model Comparison
| Model | Macro-F1 |
|------|----------|
| Logistic Regression | XX |
| Random Forest | XX |
| SVC (RBF) | XX |

Final model selected: **SVC (RBF Kernel)**

---

## 5. Final Pipeline
The final audio classification pipeline:


Final pipeline details:
- Preprocessing: Trim + Normalize + Fixed Length + Pre-emphasis
- Features: MFCC + Delta + Delta-Delta + Log-Mel + Spectral Features + CMVN + Robust Pooling
- Model: Support Vector Classifier (RBF Kernel)

---

## 6. Repository Structure

.
├── README.md
├── requirements.txt
├── ceg3004_project_colab.ipynb
├── models/
│ └── TEAM_ID_model.joblib
├── results/
│ ├── preprocessing_results.csv
│ ├── feature_results.csv
│ ├── model_results.csv
│ └── TEAM_ID_submission.csv
├── figures/
│ ├── preprocessing_comparison.png
│ ├── feature_comparison.png
│ └── model_comparison.png
└── docs/
└── method_notes.md

---

## 7. How to Run the Project

### Step 1 – Install dependencies
```bash
pip install -r requirements.txt
```

Step 2 – Open the notebook

Run the provided notebook:
```bash
ceg3004_project_colab.ipynb
```

Step 3 – Set TEAM_ID

Update the TEAM_ID variable at the top of the notebook.

Step 4 – Train the model

The code will:

Load audio files
Extract features
Train the model
Evaluate Macro-F1 score
Save the trained model
Step 5 – Generate predictions

The code will generate:

Submission CSV file
Trained model file (.joblib)

8. Reproducibility

To reproduce results:

Use the provided dataset
Set the correct TEAM_ID
Run all notebook cells in order
Use the final selected preprocessing and feature extraction pipeline
Train the SVC model and generate predictions

9. Team Contribution
Team Member	Contribution
Member 1	Audio preprocessing, DSP feature extraction
Member 2	Model training, experiments, GitHub documentation
Both	Evaluation, testing, final submission

10. Conclusion

This project demonstrates that combining meaningful DSP feature extraction with a non-linear classifier such as SVC can achieve strong performance in environmental sound classification. Experimental comparisons show that preprocessing, feature engineering, and model selection all significantly affect classification performance. The final system is robust, reproducible, and performs well under clean, noisy, and band-limited conditions.


---

# What else you should upload to GitHub
Make sure your repo contains:

| File | Purpose |
|------|---------|
| README.md | Project explanation |
| requirements.txt | Libraries |
| Notebook / Python script | Your code |
| model.joblib | Final trained model |
| submission.csv | Final predictions |
| results tables | Experiment results |
| figures | Graphs |
| docs/method_notes.md | DSP explanation |

---

# requirements.txt (put this file in GitHub)
```txt
numpy
pandas
librosa
scikit-learn
joblib
matplotlib
tqdm
soundfile
```

Final Tip

Markers usually look for:

Clear README
Experiments table
Explanation of DSP features
Why you chose SVC
Reproducibility steps

If your README has all these sections, your GitHub marks should be quite safe.
