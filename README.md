# xArm Visual Grasping & Hand-Eye Calibration System

This project implements a complete pipeline for robotic visual grasping using a **UFACTORY xArm** and an **Orbbec RGB-D camera**. It consists of two main modules:
1.  **Automatic Hand-Eye Calibration:** A script to automatically collect data and compute the transformation matrix between the camera and the robot gripper (Eye-in-Hand).
2.  **QR Code Visual Grasping:** An application that utilizes the calibration result to detect, approach, grasp, and transport objects identified by QR codes.

## 📂 Project Structure

* **`autocali.py`**: The automatic calibration script. It moves the robot in a multi-layer orbit pattern around an Aruco GridBoard to calculate the hand-eye matrix.
* **`obstacle2.py`**: The main execution script. It detects QR codes, converts coordinates from Camera Space to Base Space, and performs a pick-and-place operation with collision avoidance (smooth path).
* **`eye_in_hand_gridboard_result.npy`**: The output file from the calibration step (numpy binary). This file is required by `obstacle2.py`.
* **`qrcode_*.png`**: Sample QR code images used for testing detection.

## 🛠 Hardware Requirements

* **Robot:** UFACTORY xArm (e.g., xArm 6) with Gripper.
* **Camera:** Orbbec RGB-D Camera (mounted on the robotic arm end-effector).
* **Calibration Target:** Printed Aruco GridBoard (Dictionary: `DICT_6X6_250`, 5x7 layout).
* **Objects:** Blocks or items labeled with QR codes.

## 📦 Software Prerequisites

**1. Activate Virtual Environment**
Before installing dependencies or running the scripts, you must activate the configured virtual environment:

```bash
source ~/orbbec_arm_env/bin/activate
