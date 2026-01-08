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
2. Install DependenciesEnsure you have the required Python libraries installed:Bashpip install numpy opencv-python opencv-contrib-python scipy pyzbar xarm-python-sdk
Note: You also need the pyorbbecsdk installed and configured for your specific operating system.⚙️ ConfigurationBefore running the scripts, please check the network configuration at the top of the Python files:Robot IP Address:In autocali.py: Update ROBOT_IP = '192.168.1.228' (Default in script).In obstacle2.py: Update ROBOT_IP = '192.168.0.228' (Default in script).Ensure these match your actual robot's IP address.Offsets & Parameters:Calibration: Modify MARKER_LENGTH and MARKER_SEPARATION in autocali.py if your printed board differs.Grasping: Modify GRASP_Z_OFFSET and CAMERA_X/Y_OFFSET in obstacle2.py to fine-tune physical grabbing accuracy.🚀 Usage GuideStep 1: Hand-Eye Calibration (autocali.py)This script performs an "Eye-in-Hand" calibration. It moves the robot automatically to photograph the calibration board from multiple angles.Place the Aruco GridBoard on a flat surface directly beneath the robot.Run the script:Bashpython autocali.py
Preview Window: A window will open showing the camera feed. Press Enter when the board is clearly visible to start the automatic collection.Process: The robot will move through ~30 waypoints. Do not touch the robot during this process.Result: Upon completion, the script calculates the matrix and saves it as eye_in_hand_gridboard_result.npy.Step 2: Visual Grasping (obstacle2.py)Once calibration is complete and the .npy file is generated, you can run the grasping task.Ensure eye_in_hand_gridboard_result.npy is in the same directory.Run the script:Bashpython obstacle2.py
Workflow:The robot moves to the START position.It scans for a QR code.Once a QR code is stabilized (verified by position history), the robot calculates the 3D coordinate.It executes the Pick -> Lift -> Move Path -> Place -> Return sequence.Press q in the video window to stop the program safely.🧩 Key FeaturesLook-At Algorithm (Calibration): In autocali.py, the robot automatically rotates the end-effector to keep the calibration board centered in the image frame while moving to different X/Y/Z positions.Coordinate Transformation: Uses cv2.calibrateHandEye (Tsai method) to solve $AX=XB$.Smooth Motion: obstacle2.py uses set_servo_angle with radius parameters (Blended Motion) to create smooth, non-stop movements between waypoints during the placement phase.Safety Mechanisms:Coordinate filtering (history buffer) to prevent jitter.Z-axis safety heights (SAFETY_HEIGHT) to avoid collisions during travel.⚠️ Troubleshooting"Orbbec Camera not found": Ensure the USB cable is connected to a USB 3.0 port and the user has permission to access the device."Calibration Error High": Ensure the calibration board is perfectly flat and the lighting is consistent. Do not bump the table during calibration."Robot IP Timeout": Check if your PC is in the same subnet as the xArm. The scripts currently use different subnets (.1.x and .0.x), please verify your network settings.
