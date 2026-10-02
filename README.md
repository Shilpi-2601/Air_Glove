# SignSense: ESP32-Based Smart Glove for Real-Time Sign Language Recognition

> A low-cost wearable glove that converts hand gestures into text and voice, built for the **Innotech Hackathon**, KIET Group of Institutions, Ghaziabad.

**Team:** [Team Name]

---

## Table of Contents

1. [Problem Statement](#problem-statement)
2. [Overview](#overview)
3. [Key Features](#key-features)
4. [System Architecture](#system-architecture)
5. [Hardware](#hardware)
6. [Software](#software)
7. [Supported Gestures](#supported-gestures)
8. [Getting Started](#getting-started)
9. [Repository Structure](#repository-structure)
10. [Results](#results)
11. [Demo](#demo)
12. [Future Scope](#future-scope)
13. [Team](#team)
14. [Contribution Guidelines](#contribution-guidelines)

---

## Problem Statement

Deaf and mute individuals rely on sign language, which most people cannot understand. This communication gap creates barriers in education, healthcare, public services and daily life. Commercial sign-language gloves exist, but they are expensive and often inaccessible.

## Overview

SignSense is a wearable smart glove that uses **flex sensors** to measure finger bending and an **IMU (MPU6050)** to measure hand orientation and motion. An **ESP32** microcontroller reads and filters the sensor data and transmits it wirelessly to a computer. A machine learning classifier recognizes the gesture, and the result is displayed as **text** and spoken aloud as **voice**.

The prototype focuses on **real-time sign-language recognition** with a limited, reliable vocabulary of static and dynamic gestures.

## Key Features

- Real-time gesture recognition from flex sensor and IMU data (sensor fusion)
- Wireless data transmission using ESP32 (BLE / Serial)
- Text and voice output (English, with Hindi as an extension)
- Per-user calibration (open hand and closed fist)
- Debounce logic to prevent false triggers
- Portable, battery-powered, low-cost design

## System Architecture

```
Flex Sensors (x5) + IMU (MPU6050)
            |
            v
         ESP32  (read, filter, calibrate)
            |
            v
   Wireless link (BLE / Serial)
            |
            v
   Python: preprocessing + feature extraction
            |
            v
   ML classifier (KNN / Random Forest / SVM)
            |
            v
   Debounce + smoothing
            |
            v
   Text display + Voice output
```

**Data packet format (ESP32 to PC):**

```
f1,f2,f3,f4,f5,ax,ay,az,gx,gy,gz
```

| Field | Meaning |
|---|---|
| f1 to f5 | Flex sensor readings (thumb to little finger) |
| ax, ay, az | Accelerometer values |
| gx, gy, gz | Gyroscope values |

## Hardware

| Component | Qty | Purpose |
|---|---|---|
| ESP32 DevKit | 1 (+1 spare) | Main microcontroller with BLE/Wi-Fi |
| Flex sensor | 5 (+1 spare) | Finger bend measurement |
| MPU6050 (GY-521) | 1 (+1 spare) | Hand orientation and motion |
| 10 kOhm resistors | 5 or more | Voltage dividers for flex sensors |
| Li-Po battery (3.7 V) | 1 | Portable power |
| TP4056 charging module | 1 | Battery charging with protection |
| 5 V boost converter | 1 | Stable supply to ESP32 VIN |
| Slide switch | 1 | Power on/off |
| Stretchable glove | 1 | Wearable platform |
| 3D-printed enclosure | 1 | Housing for electronics |
| Perfboard, flexible wires, Velcro | 1 set | Assembly and sensor mounting |

**Pin mapping (ADC1 pins are used because ADC2 does not work while Wi-Fi/BLE is active):**

| Signal | ESP32 GPIO |
|---|---|
| Flex sensor 1 to 5 | 32, 33, 34, 35, 36 |
| MPU6050 SDA | 21 |
| MPU6050 SCL | 22 |

Detailed bill of materials and circuit diagram: see [`hardware/`](hardware/).

## Software

**Firmware (ESP32)**
- Arduino IDE or PlatformIO, Embedded C/C++
- Libraries: Adafruit MPU6050 (or equivalent), Wire, BLE / BluetoothSerial

**PC side (Python 3.10+)**
- `numpy`, `pandas`: data handling
- `scikit-learn`, `joblib`: model training and saving
- `matplotlib`: plots and confusion matrix
- `pyserial`, `bleak`: data reception
- `pyttsx3`: text-to-speech
- `opencv-python`, `mediapipe`: optional camera-based reference/validation

## Supported Gestures

The vocabulary is intentionally small so that accuracy stays high.

| # | Gesture / Meaning | Type |
|---|---|---|
| 1 | [fill in] | Static |
| 2 | [fill in] | Static |
| 3 | [fill in] | Dynamic |

Full list and labels: [`data/gesture_list.md`](data/gesture_list.md)

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<username>/SignSense-Smart-Glove.git
cd SignSense-Smart-Glove
```

### 2. Flash the firmware

1. Install Arduino IDE (or PlatformIO) and the ESP32 board package.
2. Install the required libraries.
3. Open `firmware/esp32_main/esp32_main.ino`, select the ESP32 board and port, and upload.

More details: [`firmware/README.md`](firmware/README.md)

### 3. Set up the Python environment

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 4. Collect data

```bash
python software/data_collection/logger.py
```

### 5. Train the model

```bash
python software/training/train_static.py
python software/training/train_dynamic.py
```

### 6. Run live recognition

```bash
python software/inference/live_predict.py
```

## Repository Structure

```
SignSense-Smart-Glove/
├── hardware/       Circuit diagram, BOM, component notes, photos
├── firmware/       ESP32 code and test sketches
├── software/       Data logger, preprocessing, training, inference, UI
├── data/           Raw and processed gesture data, gesture list
├── models/         Trained models
├── cad/            Enclosure CAD files, STL, glove material study
├── docs/           Architecture, research, results, presentation
└── tests/          Basic tests
```

## Results

> To be filled after testing.

| Metric | Value |
|---|---|
| Number of gestures | [ ] |
| Overall accuracy | [ ] % |
| Static gesture accuracy | [ ] % |
| Dynamic gesture accuracy | [ ] % |
| End-to-end latency | [ ] ms |
| Users tested | [ ] |

Confusion matrix and detailed analysis: [`docs/results.md`](docs/results.md)

## Demo

- Demo video: [link]
- Presentation: [link]

## Future Scope

- Two-hand glove and a larger Indian Sign Language vocabulary
- Mobile app for text and voice output
- On-device inference using TinyML
- Robot and IoT control using gestures
- Gaming and VR/AR interfaces
- Prosthetic and rehabilitation applications (finger movement monitoring)

## Team

| Name | Role |
|---|---|
| Shilpi | Team Leader, Integration and Firmware Support |
| [Name] | Software / ML |
| [Name] | Hardware: CAD and Glove Design |
| [Name] | Hardware: Electronics and Firmware |

**Institution:** KIET Group of Institutions, Ghaziabad
**Event:** Innotech Hackathon

## Contribution Guidelines

1. Create a branch for every task: `member-name/task` (example: `riya/mpu6050-test`).
2. Write clear commit messages describing what changed.
3. Do not push directly to `main`; open a Pull Request for review.
4. Run `git pull origin main` before starting work each day.
5. Do not commit large files (videos, large CAD exports); share them via Drive and add the link in `docs/`.

---

*Built with ESP32, flex sensors, an IMU and Python.*