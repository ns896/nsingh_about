# Power Monitor

This power monitoring system leverages the capabilities of the INA3221 power monitor supervisor IC. It can measure voltage rails relative to GND and the shunt voltage across a shunt resistor, which can be translated by an MCU into current consumption values for each voltage rail. Additionally, the system includes various alert and supervisor functions that can be configured to provide protection for circuits, ensuring robust and reliable power management.

The data rail voltage and rail current consumption on each rail is preprocessed on the STM32, and then compressed into packets which are then provided to the Host over a UART communication lines. A TUI written for this is used to visualize the data coming from the sensor.

<!-- Block Diagram of the Power Monitor -->
![System Block Diagram](Power_Monitor.png)

<!-- Table of Contents -->
# :notebook_with_decorative_cover: Table of Contents

- [1.0 Introduction](#introduction)
  * [Features](#features)
  * [Main Silicion Components](#main-silicon-components)
  * [HYDRA ML SETUP DESCRIPTION](#hydra-ml-setup-description)
    - [HYDRA MODE SELECTION](#mode-configuration)
    - [HYDRA CLOCK](#clocks-configuration)
    - [HYDRA HW MODIFICATIONS](#hydra-hw-modifications)
  * [TIMING DIAGRAMS](#timing-diagrams)
    - [HYDRA CAMERA TIMING DIAGRAMS](#radar-head-trigger-timing-diagram-calculation)
    - [HYDRA CAMERA FW STATE TIMING DIAGRAMS](#camera-fw-trigger-timing-diagram-calculation)
- [FIRMWARE](#firmware)
  * [FIRMWARE IMAGES DESCRIPTION](#firmware-images-description) 
  * [FOLDER STRUCTURES](#folder-structure)
  * [SIMULATION](#simulation)
  * [COMPILATION REPORT](#compilation-report)
- [SCOPE SNIPPETS](#scope-snippets)
- [RELEASES](#releases)  

<!-- Introduction -->
##  Introduction
<p><div align="left">
This power monitoring system leverages the capabilities of the INA3221 power monitor supervisor IC. It can measure voltage rails relative to GND, thus providing the absolute rail voltage and also the shunt voltage across a shunt resistor, which can be translated by an MCU into current consumption values for each voltage rail. Additionally, the system includes various alert and supervisor functions that can be configured to provide protection for circuits, ensuring robust and reliable power management.<br>

### Features
+ Voltage rails measurement 0-26 Volts.
+ Current Consumption on each rails.
+ Programmable Alert and Warning Outputs, on MCU GPIO interrupts.
+ Software controlled reset.
+ Averaging mode on IC upto 1024 number of samples that are averaged.

### Main Silicon Components 
+ INA3221 - (Texas Instruments)
+ STM32H743 - (STMicro)
+ FTDI USB to UART (FTDI)
+ Voltage Regulator (STMicro)
