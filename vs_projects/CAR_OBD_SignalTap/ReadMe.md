# CAR DashBoard Using OBD-II CAN BUS

This project uses an Arduino-UNO microcontroller board along with a CAN bus shield to grab messages from the OBD-II port of the CAR and then later repackage them and stream the messages to a PC using UART protocol.
this UART stream is then later decompressed and reformatted into the right message structure based on the DBC files for the CAR.

<p> <div align="justify">
Modern vehicles generate a wealth of sensor data, including speed encoder readings, steering wheel angle, and pedal actuation (gas/brake). These data points are digitally available on the CAN bus via the OBD-II interface, making them easily accessible for real-time visualization. Leveraging this information will significantly enhance Zendar’s demo setup, providing deeper insights into system performance.

Additionally, the ARS-548/542 radar object list depends on this data to generate accurate detections, enabling precise comparisons with competitors.
This will assist in addressing corner cases in radar odometry, we can improve estimations of vehicle direction and speed, leading to more reliable and robust performance in challenging scenarios by radar odometry.
</div> </p>

<!-- Block Diagram of the Power Monitor -->

![System Block Diagram](/vs_projects/CAR_OBD_SignalTap/assets/System_Diagram.png)
<p><div align="center"> IMAGE-1 - CAN BUS Sniffer Block Diagram </div> </p>


![System Block Diagram](/vs_projects/CAR_OBD_SignalTap/assets/TUI_Still.png)
<p><div align="center"> IMAGE-2 - TUI CAR DASH </div> </p>

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents

- [1.0 Introduction](#introduction)

  - [1.1 Features](#features)
  - [1.2 Main Silicion Components](#main-silicon-components)

- [2.0 Hardware Overview](#hardware-overview)

  - [2.1 MicroController Implementation](#power-supply-implementation)
  - [2.2 CAN-BUS to MCU Interface](#can-bus-to-mcu-interface)
  - [2.3 MCU to PC Interface](#cp2012n_a02_gqfn28r-implementation)
  - [2.4 OBDII to CAN-Shield Interface ](#ina3221-measuremetns-shunt-summation)

- [3.0 PC Interface](#pc-interface)

  - [3.1 Linux Bringup](#ubuntu-bringup)
  - [3.2 Windows BringUp](#)

- [4.0 Project Misc. Images](#project-images)

- [5.0 CAN Data Stream Simulator](#can-bus-uart-simulator)

## Introduction

<p><div align="justify">
Modern vehicles generate a wealth of sensor data, including speed encoder readings, steering wheel angle, and pedal actuation (gas/brake). These data points are digitally available on the CAN bus via the OBD-II interface, making them easily accessible for real-time visualization. Leveraging this information will significantly enhance Zendar’s demo setup, providing deeper insights into system performance.

Additionally, the ARS-548/542 radar object list depends on this data to generate accurate detections, enabling precise comparisons with competitors.
This will assist in addressing corner cases in radar odometry, we can improve estimations of vehicle direction and speed, leading to more reliable and robust performance in challenging scenarios by radar odometry.
</div></p>

### Features

### Main Silicon Components

<p>
MCP2551 CAN-PHY- High speed CAN Transceiver <br>
MCP2515 CAN-SPI bridge Stand alone CAN controller with SPI interface <br>
ATMEGA328P Main MCU <br>
</p>

## Hardware Overview

### CAN-BUS to MCU Interface 

<p>
This module sits on top of a microcontroller and is responsible for capturing CAN messages from the bus. It then converts these messages to UART format and transmits them over USB for further processing or analysis.
</p>

![System Block Diagram](/vs_projects/CAR_OBD_SignalTap/assets/CAN_Shield_Annotated.png)
<p><div align="justify"> IMAGE-2 - CAN BUS Shieled Diagram </div> </p>


## PC Interface

<p> <div align="justify">


### Data Flow Overview
This system captures, processes, and visualizes CAN bus data by utilizing a CAN controller, microcontroller, and PC-based TUI interface. 

The data flows through the following stages:<br>
#### CAN PHY + CAN Controller<br>
Captures raw CAN messages from the vehicle’s CAN bus.
Communicates with the microcontroller over SPI bus.

#### Microcontroller<br>
Retrieves data from the CAN buffer stored in the controller IC.
Sends the data via USB/UART bus using a baud rate of 115200 in canonical style output format.

#### X86-PC (Processing & Visualization)<br>
Captures incoming CAN messages via UART.
Filters messages based on CAN ID to extract relevant data.
Stores the filtered messages in a structured format for efficient processing.
Implements a TUI (Text-Based User Interface) to display the CAN data in real-time.

#### Additional Features<br>
Threading is utilized for efficient data capture and processing.
ncurses is used to build the TUI for an interactive visualization experience.
This setup enables real-time CAN bus monitoring, helping with debugging, and data visualization for radar processor data capture.

</div>
</p>


![System Block Diagram](/vs_projects/CAR_OBD_SignalTap/assets/data_flow.png)
<p><div align="justify"> IMAGE-2 - CAN BUS Shieled Diagram </div> </p>

## CAN BUS UART Simulator

# CAN Bus UART Simulator Setup Guide

This guide explains how to set up and run a CAN bus simulator using UART over virtual serial ports with socat.

## Prerequisites

- socat utility installed (`sudo apt-get install socat`)
- Python 3.x with pyserial library (`pip install pyserial`)

## Setup Steps

1. Create virtual serial port pair using socat:

```bash
# Creates a pair of virtual serial ports for testing
socat -d -d pty,raw,echo=0 pty,raw,echo=0

# Command explanation:
# -d -d        : Enable debug output (two levels of verbosity)
# pty          : Create a pseudoterminal device
# raw          : Set the PTY to raw mode (no special character processing)
# echo=0       : Disable local echo
# The command creates two linked PTYs that can communicate with each other
# Example output: /dev/pts/2 <-> /dev/pts/3
```

2. Run the CAN bus simulator:

```bash
python3 CAN_Simulator.py
``` 
**Configuration Notes:**

The `CAN_Simulator.py` script has several configurable parameters:

- **Serial Port**: Currently hardcoded but should be updated to match the virtual port created by socat in step 1
- **Baud Rate**: Default is 115200 but can be modified as needed
- **Input File**: The data source file path needs to be updated to your local file location

Example configuration in `CAN_Simulator.py`:


## Video Of the Cpp Dash Working
video for the TUI running :
https://youtu.be/4FiFbgb73mQ

Video of the Setup Used : 
https://www.youtube.com/shorts/QEcf5zgolIk
