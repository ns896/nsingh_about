# IMU Pose Estimator In Ros2 RViz

## Video Blogs 

## Introduction

**🎯 Real-Time 3D Pose Tracking with ROS2 & RViz**

Ever wondered how robots, drones, and VR headsets know exactly how they're oriented in 3D space? This project brings that same cutting-edge technology to life through a complete hardware-software pipeline that transforms raw sensor data into stunning real-time 3D visualizations!

**The Magic Behind the Motion:**
- **🧠 Smart Sensor Fusion**: Harnessing the power of the MPU9250 9-axis IMU (accelerometer, gyroscope, and magnetometer) to capture every twist, turn, and tilt
- **⚡ Real-Time Processing**: An ATMEGA-328P microcontroller acts as the bridge, translating sensor data over I2C and streaming it via USB-UART
- **🎨 3D Visualization**: ROS2 nodes seamlessly process the data stream, feeding RViz's powerful visualization engine to create immersive 3D pose tracking

*Watch as raw sensor data transforms into smooth, accurate 3D pose estimation in real-time—the same technology powering autonomous vehicles and advanced robotics systems.*

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents

- [1.0 Introduction](#introduction)

  - [1.1 Features](#features)
  - [1.2 Main Silicion Components](#main-silicon-components)

- [2.0 Hardware Overview](#hardware-overview)

- [3.0 PC Interface](#pc-interface)

  - [3.1 Linux Bringup](#ubuntu-bringup)
  - [3.2 Windows BringUp](#)

- [4.0 Project Misc. Images](#project-images)

- [5.0 CAN Data Stream Simulator](#can-bus-uart-simulator)

## Intro 
### Main Silicon Components 
 - MPU-9250 - MEMS Sensor Device
 - ATMEGA328P - MicroController 
 - 5V TTL_USB Cable - Protocol Converter Cable to Convert UART to USB interface


## Hardware Overview

<!-- Block Diagram of Hardware Used-->

![System Block Diagram](assets/Hardware_Block_Diagram.png)
<p><div align="center"> IMAGE-1 - Hardware Block Diagram </div> </p>
