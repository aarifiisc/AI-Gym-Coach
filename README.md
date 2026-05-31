# AI-Gym-Coach

# Automated Exercise Form Assessment via Vision-Language Models

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-Framework-ee4c2c.svg)](https://pytorch.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-Vision-5C3EE8.svg)](https://opencv.org/)

## Overview
This project tackles the challenging problem of automated human exercise form assessment. Traditionally performed by in-person trainers, proper form evaluation is critical for injury prevention and performance optimization in strength training. 

This repository introduces a multimodal framework that utilizes a Vision-Language Model (VLM) to achieve accurate pose understanding, providing contextual feedback and natural language recommendations for pose correction based strictly on visual landmark data. 

### Supported Exercises
The current scope of the project is optimized for three core exercises:
* 🏋️ **Squats**
* 🏋️ **Deadlifts**
* 💪 **Push-ups**

---

## System Architecture

The framework relies purely on visual tracking and structured keypoint representation, ensuring lightweight processing without the need for audio modalities. 

### 1. Inference Pipeline
* **Input:** Real-time webcam feed or pre-recorded workout video.
* **Vision Processing:** A Human Pose Estimation (HPE) model extracts body landmark keypoints on a per-frame basis.
* **Feature Transformation:** Keypoints are projected into a structured numerical representation.
* **VLM Feedback:** The transformed features are passed to a pre-trained Language Model, which generates detailed pose descriptions and contextual instructions (e.g., *"keep your back straight," "align your knees with your toes"*).

### 2. Training Pipeline
* **Dataset:** A highly curated dataset of annotated workout videos. Annotations are initially generated via automated labeling techniques and subsequently verified/refined manually to ensure a high-quality ground truth.
* **Optimization:** We utilize Parameter-Efficient Fine-Tuning (PEFT) on both the pose representations and the language model.
* **Generalization:** Instruction-based fine-tuning is leveraged to help the model generalize effectively even with limited data, learning the complex alignment between structured keypoints and natural language feedback.

---

## Installation

Clone the repository and install the required dependencies. It is recommended to use a virtual environment.

```bash
# Clone the repository
git clone [https://github.com/YourUsername/Exercise-Form-VLM.git](https://github.com/YourUsername/Exercise-Form-VLM.git)
cd Exercise-Form-VLM

# Create and activate a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`

# Install core dependencies (PyTorch, OpenCV, Transformers, etc.)
pip install -r requirements.txt
```

---

## Usage

### Running Inference
To run the automated assessment on a pre-recorded video:

```bash
python inference.py --input path/to/workout_video.mp4 --exercise squat
```

To run real-time inference using your webcam:

```bash
python inference.py --input webcam --exercise deadlift
```

### Training / Fine-Tuning
To initiate the PEFT pipeline on your custom pose-to-text dataset:

```bash
python train.py --config configs/peft_config.yaml
```

---

## Dataset Annotation
If you are contributing to the dataset, please refer to our `docs/ANNOTATION_GUIDE.md`. The process involves:
1.  Running the baseline HPE script to generate initial bounding boxes and keypoints.
2.  Using our provided UI to manually verify and adjust the keypoints.
3.  Pairing the verified keypoint sequences with textual feedback instructions.

---
