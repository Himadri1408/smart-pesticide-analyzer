# Smart Non-Invasive Spectroscopic Pesticide Analyzer with Machine Learning-Based Detection and Automated Cleaning

## Project Overview
This project introduces a **Non-Destructive Intelligent Pesticide Residue Detection System** based on ultraviolet/infrared (UV/IR) spectroscopy. The device is designed to identify pesticide contamination in fruits and vegetables in real-time, offering a portable and cost-effective alternative to traditional laboratory methods like chromatography.

### Key Features
* **Non-Invasive Sensing**: Uses dual-wavelength spectroscopy (365 nm UV and 940 nm IR) to identify chemical "fingerprints" without damaging the food.
* **Edge AI Classification**: Features a quantized Tiny Machine Learning (TinyML) Multi-Layer Perceptron (MLP) model deployed on-device for rapid classification.
* **Automated Cleaning**: Includes a 40-second time-sequenced cleaning mechanism using mist generation and forced airflow to prevent cross-contamination.
* **IoT Integration**: Connects via Wi-Fi to a web dashboard for remote monitoring and real-time alerts.

---

## Hardware Architecture
The system is centered around a dual-microcontroller setup in a light-shielded optical chamber.

* **Microcontrollers**: 
    * **STM32F411CEU6**: Handles primary signal acquisition, processing, and hardware control.
    * **ESP32-WROOM**: Manages Wi-Fi connectivity, data transmission to the IoT dashboard, and edge AI inference.
* **Sensors**:
    * **UV Sensor (GUVA-S12SD)**: Detects reflected light from the 365 nm UV LED.
    * **IR Sensor (BPW41N)**: Detects reflected light from the 940 nm IR LED.
* **Actuators (Cleaning System)**:
    * **Ultrasonic Mist Generator**: Uses a neutralizing baking soda solution.
    * **Fans (5V & 12V)**: Facilitate mist distribution and high-speed drying.
* **Display**: OLED screen for local status updates.

---

## Software & Machine Learning
### TinyML Pipeline
1.  **Data Acquisition**: Captures normalized spectral data (UV, IR, and white-light).
2.  **Model**: A compact MLP neural network captures non-linear spectral characteristics.
3.  **Optimization**: The model is quantized to 8-bit integers (int8) and converted to TensorFlow Lite Micro format for edge deployment.
4.  **Performance**: Achieved approximately 95–100% classification accuracy with zero misclassifications in testing.

### Automated Cleaning Sequence (40 Seconds)
* **0-10s (Initial Mist)**: Baking soda solution lifts residue.
* **10-15s (Airflow)**: 5V fan mixes the mist evenly.
* **15-20s (Secondary Injection)**: Clean water rinse.
* **20-30s (Partial Drying)**: Steady fan airflow begins moisture removal.
* **30-40s (Final Evacuation)**: 12V fan surges to dry the chamber completely for the next test.

---

## Methodology
The system follows a four-step process: **Sensing, Processing, Cleaning, and Communication**.
1.  The sample is placed in the optical chamber and illuminated with UV/IR light.
2.  Photodiodes capture reflected light, converted into voltage signals by the STM32.
3.  If pesticides are detected, the cleaning cycle is triggered automatically.
4.  Data is sent via ESP32 to the IoT dashboard, which features color-coded status alerts (e.g., "FOOD CLEAN" or "DETECTED").

---

## Results
* **Selectivity**: Emamectin Benzoate showed strong UV absorption (approx. 19.08% signal reduction), while Lambda-Cyhalothrin showed distinct IR reflectance variations.
* **Efficiency**: Detection time was reduced from hours (laboratory standard) to seconds.
* **Reliability**: The system includes a re-detection loop after cleaning to verify the chamber is free of pollutants.
