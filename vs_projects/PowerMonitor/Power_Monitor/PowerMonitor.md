# Power Monitor

This power monitoring system leverages the capabilities of the INA3221 power monitor supervisor IC. It can measure voltage rails relative to GND and the shunt voltage across a shunt resistor, which can be translated by an MCU into current consumption values for each voltage rail. Additionally, the system includes various alert and supervisor functions that can be configured to provide protection for circuits, ensuring robust and reliable power management.

The data rail voltage and rail current consumption on each rail is preprocessed on the STM32, and then compressed into packets which are then provided to the Host over a UART communication lines. A TUI written for this is used to visualize the data coming from the sensor.