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
