# Power Monitor

This power monitoring system leverages the capabilities of the INA3221 power monitor supervisor IC. It can measure voltage rails relative to GND and the shunt voltage across a shunt resistor, which can be translated by an MCU into current consumption values for each voltage rail. Additionally, the system includes various alert and supervisor functions that can be configured to provide protection for circuits, ensuring robust and reliable power management.

The data rail voltage and rail current consumption on each rail is preprocessed on the STM32, and then compressed into packets which are then provided to the Host over a UART communication lines. A TUI written for this is used to visualize the data coming from the sensor.

<!-- Block Diagram of the Power Monitor -->
![System Block Diagram](Power_Monitor.png)

<!-- Table of Contents -->
# :notebook_with_decorative_cover: Table of Contents

- [1.0 Introduction](#introduction)
  * [1.1 Features](#features)
  * [1.2 Main Silicion Components](#main-silicon-components)

- [2.0 System Overview](#system-overview)
    * [2.1 INA3221 Measurements BUS Voltage](#ina3221-measurements-bus-voltage)
    * [2.2 INA3221 Measurements SHUNT Voltage](#ina3221-measuremets-shunt-voltage)
    * [2.3 INA3221 Measurements SHUNT SUMMATION](#ina3221-measuremetns-shunt-summation)

- [3.0 STM32H7 Data Flow](#timing-diagrams)
    - [3.1 UART Packet Encoding and Decoding Scheme](#radar-head-trigger-timing-diagram-calculation)
    - [3.2 UART Properties Sender Side](#camera-fw-trigger-timing-diagram-calculation)
    - [3.2 UART Properties Receiver Side](#camera-fw-trigger-timing-diagram-calculation)

- [4.0 (x86) Linux Application](#firmware)
- [5.0 Hardware Design & Other Constrains](#firmware)
- [6.0 Conclusion](#firmware)

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

## System Overview
INA3221 is used to measure the various voltages and also performs the analog to digital conversion for the various systems measurements that are needed for the power monitoring and supervision purpose. Various native measurements that are performed on the INA3221 IC itself are Bus_Voltage, Shunt_Voltage and Shunt_Voltage_Sum.

The Bus_Voltage is simply a ADC reading on the IN_1,IN_2,IN_3 pins of the INA3221 IC. This bus voltage can only go 26_Volts on that pin. This voltage can be measured with a accuracy of 8mV, this is driven by the ADC of the INA3221.

The Shunt_Voltage is a difference voltage reading on the two ends of the SHUNT_Resistor. Voltage measurements on pins VIN+1__VIN-1, VIN+2__VIN-2, VIN+3__VIN-3. The voltage difference and the accurate value of resistance can be used to calculate the current flowing through that rail.

The per channel voltages can be summed up or averaged by the INA3221 FSM itself. The configration register space 11-9 bit, can be used to define how many samples are summed up to make a valid reading. Values include 1,4,16,64,128,1024. Default value is 1, I have not played with it much to see the effect. But the mathematical implications are Lower Noise and Accuracy improvement.

The INA3221 communicates with the MCU on the I2C bus. The max operating frequency for the SCL line is 2.44MHz. This is only the case for the high speed mode which needs a repeated start condition for entering a high speed communication mode. in Fast/Normal mode the maximum frequency of communication is not more then 0.4MHz(400Khz).

### INA3221 Measurements BUS Voltage
The BUS_VOLTAGE is stored in the register space at addresses 0x02, 0x04, and 0x06 for the respective channels. To convert the ADC digital reading to a floating-point value, follow these steps:

Right shift the ADC reading by 3 bits.
Multiply the result by 8mV to obtain the absolute voltage.
For the code implementation, refer to the following GitHub link: INA3221 Code Line on GitHub

### INA3221 Measurements SHUNT Voltage
The BUS_VOLTAGE is stored in the register space at addresses 0x01, 0x03, and 0x05 for the respective channels. To convert the ADC digital reading to a floating-point value, follow these steps:

Right shift the ADC reading by 3 bits.
Multiply the result by 40uV to obtain the absolute voltage.
Divide the value by the resistance of the SHUNT resistor on PCB (value in Amps)
Multiply the value by 1000 (value in mAmps)
For the code implementation, refer to the following GitHub link: INA3221 Code Line on GitHub

