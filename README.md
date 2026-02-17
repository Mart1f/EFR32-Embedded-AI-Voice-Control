# Embedded Voice Command Recognition on ARM Cortex-M33 (EFR32MG24)

## Overview

This project implements a real-time embedded voice command recognition system running on an ARM Cortex-M33 microcontroller (EFR32MG24), developed using Simplicity Studio on Linux Mint.

The system detects simple voice commands ("yes" / "no" or "on" / "off") and controls GPIO outputs (LED) based on the recognized command. All inference is executed fully on-device using TensorFlow Lite Micro.

Two different machine learning deployment strategies were implemented and compared:

- Edge Impulse automated deployment (Web)
- Custom Jupyter-trained TensorFlow Lite Micro integration (self-trained TensorFlow model)


This project demonstrates embedded firmware development, RTOS task management, real-time signal processing, and on-device machine learning.

---

## Hardware Platform

- Microcontroller: EFR32MG24 (ARM Cortex-M33)
- Development Environment: Simplicity Studio
- RTOS: Micrium OS
- Audio Input: Digital microphone
- Output: LED (GPIO control)

---

## System Architecture

Processing pipeline:

1. Audio acquisition  
2. Feature generation (MFCC / spectrogram)  
3. TensorFlow Lite Micro inference  
4. Output post-processing (smoothing + threshold + suppression)  
5. GPIO control (LED ON/OFF)  

The implementation includes:

- RTOS task scheduling using Micrium OS
- Power management configuration (EM1 requirement for microphone sampling)
- TensorFlow Lite Micro interpreter integration
- Real-time inference execution
- Command recognition smoothing logic

---

## Model Deployment Approaches

### 1️⃣ Edge Impulse Deployment

Workflow:

- Dataset uploaded to Edge Impulse 745 data - Training (611) / Test (134)
- Cloud-based feature extraction and model training
- Model exported as embedded firmware (.hex)
- Firmware flashed directly to the microcontroller

Characteristics:

- Rapid prototyping
- Automated optimization for embedded systems
- Simplified deployment workflow
- Limited low-level control over model architecture

---

### 2️⃣ Custom Jupyter + TensorFlow Lite Micro Deployment

Workflow:

- Dataset preprocessing and model training performed in Jupyter Notebook (Final_code_model_yes_no_with_tflite)
- Model trained using TensorFlow
- Conversion to TensorFlow Lite format
- Integration into firmware using TensorFlow Lite Micro
- Manual invocation of inference inside the RTOS task

Characteristics:

- Full control over architecture and hyperparameters
- Custom preprocessing pipeline
- Direct firmware-level integration
- Deeper understanding of embedded memory and constraints

This approach demonstrates low-level embedded ML integration beyond automated platforms.

---

## Embedded Implementation Details

- RTOS task created using Micrium OS
- Periodic execution using OS delay functions
- Audio feature buffer updated before each inference
- TFLite Micro interpreter manually invoked
- Post-processing logic implemented to:
  - Average inference results over time
  - Apply detection thresholds
  - Prevent repeated triggering (suppression window)

Command behavior:

- "yes" → LED ON  
- "no" → LED OFF  

---

## Skills Demonstrated

- Embedded C/C++ firmware development
- ARM Cortex-M microcontroller programming
- RTOS task management
- Real-time signal processing
- TensorFlow Lite Micro integration
- On-device ML inference
- Edge Impulse deployment workflow
- GPIO hardware control
- Embedded power management

---

## Demonstration & Testing

A live demonstration of the system running on the EFR32MG24 development board is available below:

🎥 Demo Video: [Watch the embedded voice recognition demo](https://drive.google.com/file/d/1H0-HGg7328QaLVfQQSd-1nvod7bsHxjc/view?usp=sharing)

### Test Description

The system was tested under real-time conditions using spoken voice commands:

- "yes" / "on" → LED turns ON  
- "no" / "off" → LED turns OFF  

The inference task runs periodically every 200 ms within an RTOS task.  
Smoothing logic is applied to avoid false positives and repeated triggers.

Testing validated:

- Real-time audio acquisition
- Stable on-device inference
- Reliable GPIO actuation
- Proper suppression window behavior
- Consistent detection threshold performance

The system operates entirely on-device without external processing or cloud inference.


---

## Project Structure

