# Project Demo: Smart Non-Invasive Spectroscopic Pesticide Analyzer

This document provides a detailed walkthrough of the system's operational flow, showcasing how it integrates spectroscopy, edge AI, and automated remediation.

## 📺 System Demonstration
**[Click Here to Watch the Demo Video](PASTE_YOUR_DRIVE_LINK_HERE)**

> **Note:** The video demonstrates the transition from detection to the autonomous 40-second cleaning cycle.

---

## 🔍 Key Operational Phases

### 1. Optical Sensing & Detection
The system uses a non-destructive dual-wavelength approach to identify chemical "fingerprints" on produce.
* **Spectral Interrogation**: The device utilizes 365 nm UV and 940 nm IR LEDs to illuminate the sample.
* **Response Capture**: Photodiodes (GUVA-S12SD and BPW41N) measure the reflected light intensity, which is then converted into digital voltage signals by the STM32F411CEU6 microcontroller.
* **Absorbance Profiles**: Emamectin Benzoate is identified by a distinct voltage drop in the UV spectrum (approx. 19.08%), while Lambda-Cyhalothrin is identified via IR reflectance variations.

### 2. Edge AI Classification (TinyML)
Instead of relying on simple thresholds, the system runs local inference for higher precision.
* **Neural Network**: A quantized Multi-Layer Perceptron (Tiny-MLP) model processes the spectral features.
* **Real-Time Inference**: The model is optimized for the ESP32 and STM32 architecture to categorize samples as "Clean" or contaminated with ~95–100% accuracy.

### 3. IoT Monitoring Dashboard
The demo highlights the remote monitoring capabilities enabled by the ESP32-WROOM module.
* **Live Alerts**: Raw spectral data and classification results are pushed to a web dashboard.
* **Color-Coded Feedback**: The interface displays "FOOD CLEAN" or specific pesticide detections with instant warnings for chemical contamination.

### 4. Automated 40-Second Cleaning Sequence
When pesticides are detected, the system initiates a multi-stage remediation cycle to ensure repeatability and safety:
* **0–10s**: Ultrasonic misting with a neutralizing baking soda solution lifts surface residue.
* **10–15s**: A 5V fan facilitates even distribution of the neutralizing mist.
* **15–20s**: Clean water rinse to remove neutralizing agents.
* **20–40s**: A high-speed 12V fan ensures the chamber and sample are completely dry before the next test.
