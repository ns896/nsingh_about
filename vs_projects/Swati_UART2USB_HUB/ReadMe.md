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

  - [2.1 TUSB2036VFR Implementation](#tusb2036vfr-implementation)
  - [2.2 CP2012N-A02-GQFN28R Implementation](#cp2012n_a02_gqfn28r-implementation)
  - [2.3 INA3221 Measurements SHUNT SUMMATION](#ina3221-measuremetns-shunt-summation)

- [3.0 STM32H7 Data Flow](#stm32h7-data-flow)

  - [3.1 UART Packet Encoding and Decoding Scheme](#uart-packet-encoding)
  - [3.2 UART Properties Sender Side](#)
  - [3.2 UART Properties Receiver Side](#)

- [4.0 (x86) Linux Application](#linux-application)
- [5.0 Hardware Design & Other Constrains](#hardware-design)
  - [5.1 INA3221 Hardware Design](#ina3221-breakout-board)
- [6.0 Conclusion](#firmware)

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

### TUSB2036VFR Implementation

TUSB2036VFR Implementation

### CP2012N_A02_GQFN28R Implementation

USB to UART Bridge Implemetation

## STM32H7 Data Flow

The Firmware on STM32H7 is responsible for sending the data from the MCU to the main PC, on the UART bus. The BAUD RATE of the UART is 115200 Bits/second (needed to set up the host).

### UART PACKET ENCODING

The data coming out of the MCU will be in a form of data packets. Protocol that will be followed by the sender will be as follows.

SOF -> Data_0.1 -> Data_0.2 -> Data_1.0 -> Data_1.2 ......... -> EOF Each data packet is consisting of 16 bits of channel data. The protocol will have 6 channels, three for shut voltage and three for bus voltage.

```bash
SOF : Start of Frame == 0x02
BYTE1 : DATA
BYTE2 : DATA
BYTE3 : DATA
BYTE4 : DATA
...
BYTE(n-2) : DATA
BYTE(n-1) : DATA
EOF (n) : END of Frame == 0x3

```

SOF EOF CONFLICT RESOLUTION
SOF/EOF Conflict Resolution When the data to be transmitted conflicts with the Start of Frame (SOF) or End of Frame (EOF) bytes, we will use a byte stuffing option. A special character will be transmitted before the conflicting byte, which will be an XOR of the conflict byte with the special character.

- Special Character: 0x1B
  <!-- Block Diagram of the Encoding Data Protocol -->
  ![System Block Diagram](UART_DATAPACKET.png)

## Hardware Design

The hardware part of the design can be divided into two subsections, part onw which deals with the implementation of INA3221 IC and the surrounding support needed by it, i.e. power supply, shunt resistors and decoupling caps. The second part of the hardware design is the implementation of the ARM MCU and its connection to the INA3221. for this implementation I am using the ARM development board from STM32H7.

### INA3221 Breakout Board

<p style="float: left; margin-right: 10px;">
    <img src="Schematic_Snippet.png" alt="System Block Diagram" width="200" />
</p>

The following schematic board is used foe the implementation of the current sensor.

In the following board implementation the A0 pin of the INA3221 is tied to GND hence the device address is 0b1000000. This value is used to address the slave over the I2C bus.

<div style="clear: both;"></div>

## Linux Application

<!--Power Monitor Breakout Board Schematic Snippet -->

![System Block Diagram](DATA_PACKETS_UART.png)
