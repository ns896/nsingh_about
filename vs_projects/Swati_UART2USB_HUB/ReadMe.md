# Swati V1.0.0 USB to UART MUX

This project leverages the TUSB2036 USB hub IC to create a versatile 8-channel USB-to-UART interface, specifically designed to facilitate communication between a Jetson Nano(any SoC with USB2.0 or higher capability) and FPDLink ICs. By cascading four TUSB2036 hubs, the board provides multiple UART channels, enabling seamless configuration and setup of FPDLink devices during boot-up. This solution offers a compact, efficient way to manage multiple communication links, ensuring reliable and easy integration for embedded systems involving high-speed video transmission or other serial communication applications.

<!-- Block Diagram of the Power Monitor -->

![System Block Diagram](/vs_projects/Swati_UART2USB_HUB/assets/Swati_BD.png)

<p><div align="center"> IMAGE-1 SWATI - Block Diagram </div> </p>

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents

- [1.0 Introduction](#introduction)

  - [1.1 Features](#features)
  - [1.2 Main Silicion Components](#main-silicon-components)

- [2.0 System Overview](#system-overview)

  - [2.1 Power Supply Implementation](#power-supply-implementation)
  - [2.2 TUSB2036VFR Implementation](#tusb2036vfr-implementation)
  - [2.3 CP2012N-A02-GQFN28R Implementation](#cp2012n_a02_gqfn28r-implementation)
  - [2.4 INA3221 Measurements SHUNT SUMMATION](#ina3221-measuremetns-shunt-summation)

- [3.0 BringUp KRV-PA-0101-01](#bringup-software)

  - [3.1 Linux Bringup](#ubuntu-bringup)
  - [3.2 Windows BringUp](#)

- [4.0 Project Misc. Images](#project-images)

<!-- Introduction -->

## Introduction

<p><div align="left">
The Swati USB-to-8-Channel UART board was designed to provide seamless UART communication through a single USB connection on the Jetson Xavier AGX platform. This board connects to the available USB port and offers 8 UART channels, interfacing with FPD-Link serializer and deserializer ICs (DS90UB954-Q1) for reliable, multi-channel communication.

This UART communication is achieved over the FPD-Link back channel, which is essential for configuring and managing the FPD-Link serializers. By providing a single USB-to-UART interface for up to 8 LVDS data streams, this board significantly simplifies communication and setup on the back channel, making it an efficient solution for multi-channel data serialization and capture.

The board is designed to operate independently, powered by an external 12V supply via a barrel connector, preventing USB power overload and keeping within the 500mA current limitation of USB 2.0.

</p></div>

### Features

:maple_leaf:

- Inependent of bus volatge, powerded from 12V DC supply.
- One signle USB 2.0 host connection.
- 8 parallel USB-UART bridges avaiable.
- Each bridge has multiple GPIOs available, configrable from USB host side.
- Hard Reset through push button.
- High Side switchs for protection and power cycling.

### Main Silicon Components

- TUSB2036VFR - USB MUX Bridges (Texas Instruments)
- TPS2044AD - High Side Switch (Texas Instruments)
- CP2012N-A02-GQFN28R - USB to Uart bridge (Silicon Labs)

## System Overview

The Swati USB-to-8-Channel UART board operates on the concept of USB downport multiplexing to efficiently manage multiple UART channels through a single USB connection. At the heart of this system is the TUSB2036 USB hub controller, which uses a finite state machine (FSM) to multiplex a single upstream USB port into three downstream USB 1.1-compliant ports. Each TUSB2036 chip provides three additional downstream ports while maintaining compatibility with the USB 2.0 specification for the upstream port.

To achieve the required number of UART channels, four TUSB2036 controllers are configured in parallel, enabling up to nine downstream USB ports from the main USB 2.0 interface. Each of these downstream ports is connected to a dedicated USB-to-UART bridge IC (the CP2102N-A02-GQFN28R), which converts the USB data into UART communication. This arrangement provides robust, independent UART channels that interface seamlessly with each of the FPD-Link serializers and deserializers (DS90UB954-Q1 ICs) in the system, facilitating reliable communication over each UART line.

Each CP2102N USB-to-UART bridge channel provides standard UART features, including configurable baud rates, parity, stop bits, and flow control, allowing for flexible integration with a variety of serializer and deserializer configurations. By consolidating all UART channels through USB, this board minimizes the number of physical connections needed to establish communication, simplifying wiring complexity and improving manageability.

The upstream USB port on the Swati board adheres to USB 2.0 specifications, ensuring compatibility with a wide range of USB host devices. Additionally, the board's design leverages an external 12V power supply, ensuring it remains within the 500mA current limit of USB 2.0 by not drawing power directly from the USB bus. This self-powered setup provides stable operation for all connected UART channels without risk of overloading the host USB port.

![SWATI PCB Overview](/vs_projects/Swati_UART2USB_HUB/assets/Swati_PCB.png)

<p><div align="center"> IMAGE-2 SWATI PCB MarkDown </div> </p>

### Power Supply Implementation

Power Supply Implementation
The system is powered through a barrel connector with a 2.05mm diameter and a center pin configured as positive. To ensure protection and reliability, the power input includes a 1.5A PTC (Positive Temperature Coefficient) fuse on the main line. This fuse provides overcurrent protection, resetting automatically when the fault condition is removed.

A polarity protection circuit, implemented using a P-channel MOSFET (PMOS), safeguards the system from damage caused by incorrect polarity connections. This circuit allows current flow only when the correct polarity is applied, ensuring safe operation in all conditions.

The onboard power supply uses a high-efficiency switching buck regulator to step down the main input voltage from the barrel connector to regulated outputs of 3.3V and 5.0V. These voltages power various components and subsystems of the board, ensuring stable and reliable operation.

The use of a buck regulator minimizes power loss compared to linear regulators, making the system efficient even under higher loads. This design ensures sufficient current delivery for the UART bridges, USB hub controllers, and other circuit components, while maintaining thermal efficiency and protecting the system from voltage fluctuations.

<p><div align="center"> <font size="3"> IMAGE-2 SWATI Power Section </font> </div> </p>

![SWATI PowerSupply](/vs_projects/Swati_UART2USB_HUB/assets/Swati_PowerSection.png)

<p><div align="center"> <font size="3"> IMAGE-2 Scope Capture 3V3 nad 5V0 Rails </font> </div> </p>

![SWATI PowerSupply](/vs_projects/Swati_UART2USB_HUB/assets/Power_Scope_Capture.png)

### TUSB2036VFR Implementation

### CP2012N_A02_GQFN28R Implementation

USB to UART Bridge Implemetation

## Bringup Software

The Swati system is compatible with both Windows and Linux operating systems. 
On Linux, specifically Ubuntu, the drivers for the USB-to-UART bridge are prebuilt into the kernel, 
ensuring seamless plug-and-play functionality without requiring additional installation. For Windows, 
Silicon Labs provides dedicated drivers for the USB-to-UART bridge, which can be easily downloaded and 
installed to enable full compatibility and functionality.

### Ubuntu Bringup
On plugging the power and the USB port into a Ubuntu Machines the USB-Uart bridges and the USB mux briges are recognized.

+ Lines with Texas Instruments part depicts the USB Multiplexing silicon.
+ Lines with Silicon Labs depicts the USB-UART bridge silicon.

<p><div align="center"> <font size="3"> IMAGE-Misc.1 SWATI PCB MarkDown </font> </div> </p>

<p><div align="center">
  
  ![System Block Diagram](/vs_projects/Swati_UART2USB_HUB/assets/Swati-Enumeration.png)

</div> </p>

#### Useful Linux Commands 

 + Confirm Device Detection<br>
    Run the following commands to check if the UART device is properly detected:
    ```bash
    ls /dev/ttyUSB*
    dmesg | grep ttyUSB
    ```
  + Check Permissions<br>
    Ensure the current user has access to the device. Add your user to the dialout group.<br>
    Then log out and log back in.
    ```bash
      sudo usermod -aG dialout $USER
    ```
    user should be the user name

  + Check Current Permissions
    Run the following command to check the permissions of the device:
    ``` bash
    ls -l /dev/ttyUSB7
    ```
  + Check with udevadm
    The udevadm command can provide detailed information about the port:
    This shows attributes such as vendor ID, product ID, serial number, driver, and more.
    ```bash
    udevadm info -a -n /dev/ttyUSB0
    ```
  + Serial Port Configuration with stty<br>
    To view and modify the port settings, use:
    ```bash
    stty -F /dev/ttyUSB0
    ```
  + Check with udevadm<br>
    The udevadm command can provide detailed information about the port:
    This shows attributes such as vendor ID, product ID, serial number, driver, and more.
    ```bash
    udevadm info -a -n /dev/ttyUSB0
    ```

## Hardware Design


### xxx

## Linux Application


## Project Images

<p><div align="center"> <font size="5"> IMAGE-Misc.1 SWATI PCB MarkDown </font> </div> </p>

![System Block Diagram](/vs_projects/Swati_UART2USB_HUB/assets/Swati_Probing_Setup.jpeg)
