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

- [3.0 MCU PC Interface](#mcu-pc-interface)


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

## MCU PC Interface

### 🔌 Communication Protocol

The ATMEGA-328P interfaces with the PC using a **5V TTL-USB bridge** at a baud rate of **115,200 bits/second**. This creates a reliable serial communication channel for streaming real-time sensor data.
<br> `ToDo :` Later I want to run the same data over the `usb-uart` bridge using a ``ModBus``  protocol to better deal with data corruption over transmission.

### 📊 Data Format & Structure

The microcontroller outputs **JSON-formatted sensor data** containing all 9-axis measurements plus temperature:

```json
{
  "accel": {"x": 0.12, "y": -0.05, "z": 1.02},
  "gyro": {"x": 0.15, "y": -0.23, "z": 0.08},
  "mag": {"x": 25.4, "y": -18.7, "z": 42.1},
  "temp": 23.5
}
```

### ⚙️ Sensor Configuration

**MPU9250 Setup:**
- **Accelerometer Range**: ±2g (high precision for orientation)
- **Gyroscope Range**: ±250°/s (optimal for pose tracking)
- **Magnetometer**: 100Hz continuous mode
- **Digital Low Pass Filter**: Level 6 (lowest noise, 5Hz bandwidth)
- **Sample Rate**: 1kHz with 15x divider = ~66Hz effective rate

### Firmware Features

**Smart Calibration:**
- Automatic offset compensation for flat positioning
- Gyroscope drift correction
- Temperature-compensated readings

**Optimized Performance:**
- **DLPF Level 6**: Achieves lowest noise levels
- **100Hz Magnetometer**: Balanced speed vs. power consumption
- **JSON Output**: Easy parsing for ROS2 nodes
- **1ms Update Rate**: Real-time responsiveness



*This interface demonstrates the complete hardware-software bridge from raw sensor data to structured, real-time data streams ready for advanced robotics applications.*

<!-- Block Diagram of Hardware Used-->

![System Block Diagram](assets/UART_USB_Bridge_minicom_Snap.png)
<p><div align="center"> IMAGE-2 - A Still from MiniCom Terminal</div> </p>
