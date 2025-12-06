# 🎯 STM32 Sensor Fusion Robotic Arm Control (Real-Time Object Tracking)

This project implements embedded software and AI detection for a **5 Degrees of Freedom (DOF) robotic arm** controlled by an STM32F407 microcontroller. The goal is to enable the arm to autonomously **track** a designated object and **position itself** at a precise target distance using a **sensor fusion** approach.

## 💡 Project Goal

The primary objective is to use **Image Processing (Camera)** data to lock the arm's horizontal (Pan) and vertical (Tilt) axes onto the target, while simultaneously using data from an **Ultrasonic Distance Sensor** to control the arm's reach (Proximity) to maintain a fixed distance (e.g., 5 cm) from the object.

## ✨ Key Control Mechanisms

The control is executed in the main loop using **non-blocking, step-by-step** functions with interrupts that continuously adjust the servo angles based on error feedback.

* **Servo 6 (Pan - Horizontal):** Controlled by the **DX (Horizontal Deviation)** data from the camera. It centers the arm over the target horizontally.
    * *Function:* `ServoFollow6()`
* **Servo 3 (Tilt - Vertical):** Controlled by the **DY (Vertical Deviation)** data from the camera. It centers the arm over the target vertically.
    * *Function:* `ServoFollow3()`
* **Servo 5 (Proximity - In/Out):** Controlled by the **Ultrasonic Sensor Distance** reading. It constantly adjusts the arm's reach to maintain the desired target distance.
    * *Function:* `ServoFollow5()`

## ⚙️ Implementation Details

* **Control Type:** Incremental movement control (smooth stepping). Movement speed scales with the size of the error.
* **Input:** Camera (Receiving DX, DY coordinates via UART) and Ultrasonic Sensor (TIM Input Capture).
* **Output:** PWM control for the Servo motors.
* **Software:** Developed using C language and STM32 HAL Drivers.

## 🛠️ Hardware and Tools Used

This project involves a sensor fusion architecture, utilizing the processing power of a single-board computer for vision and a microcontroller for precise motor control. 

* **Microcontroller:** **STM32F407VGT6** Development Board (Handles PWM generation and Ultrasonic timing).
* **Vision Processing Unit (VPU):** **Raspberry Pi 5** (Used for running the vision algorithm, detecting the object, and sending the processed DX/DY coordinates to the STM32 via UART/Serial Communication).
* **Actuators (Servos):** **6x MG996R** Metal Gear Servos (Used for moving the joints of the robotic arm).
* **Camera Input:** **1x USB Camera** (Provides the visual feed to the Raspberry Pi for object detection).
* **Distance Sensor:** **1x HC-SR04** Ultrasonic Sensor (Provides real-time distance measurements for the proximity control of Servo 5).
* **Interface:** **1x Logic Level Converter** (Essential for safe communication between the 3.3V STM32/Raspberry Pi logic and other components, if needed, or specifically for connecting the Raspberry Pi (1.8V/3.3V) to 5V servos or sensors if not directly compatible).
