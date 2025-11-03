# Wearable Wireless Sensor System Setup Guide

This document provides instructions on how to assemble a single wearable wireless sensor unit using the specified components.

## Overview

The system consists of a microcontroller (Arduino Nano) that reads data from a 6-axis motion sensor (MPU-6050). This data is then transmitted wirelessly using a 2.4GHz transceiver (nRF24L01+). The entire unit is powered by a rechargeable Lithium Polymer (LiPo) battery, managed by a charging and boost converter module.

## Required Equipment (for one sensor unit)

| Quantity | Component                                                                                                                                                                                          | Purpose            |
| :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------- |
| 1        | **Arduino Nano V3.0 (ATmega328P)**                                                                                                                                                                 | Microcontroller    |
| 1        | **MPU-6050 GY-521 Module**                                                                                                                                                                         | 6-Axis Gyro/Accel  |
| 1        | **nRF24L01+ Wireless Module**                                                                                                                                                                      | Wireless Transceiver |
| 1        | **LiPo Battery Charger/Boost Module (134N3P)**                                                                                                                                                     | Power Management   |
| 1        | **3.7V 1000mAh LiPo Battery (803040)**                                                                                                                                                               | Power Source       |

## Additional Equipment & Considerations

*   **Connecting Wires:** Jumper wires (male-to-female, female-to-female) for prototyping.
*   **Soldering Iron & Solder:** For creating permanent, reliable connections.
*   **Breadboard:** Highly recommended for initial prototyping and testing before soldering.
*   **Receiver Unit:** To receive the data from the wearable sensor, you will need a second set of components:
    *   1 x Arduino Nano
    *   1 x nRF24L01+ Wireless Module
    *   A computer to connect the receiver Arduino to for data visualization or logging.
*   **3.3V Adapter for nRF24L01+ (Optional but Recommended):** The nRF24L01+ is sensitive to power fluctuations. A dedicated 3.3V adapter board that plugs into the breadboard can provide cleaner power than the Arduino's 3.3V pin, improving stability.
*   **Enclosure:** A 3D printed case or a small project box to house the components and protect them.
*   **Strap/Attachment:** An elastic strap, velcro, or other method to attach the unit to the body.

## Connection Instructions

**Important:** Ensure all power is disconnected before making any connections. The nRF24L01+ is a **3.3V device**. Connecting its VCC pin to 5V can permanently damage it.

### 1. Power System Connections

1.  Connect the **LiPo Battery** to the **Charger/Boost Module**.
    *   Battery **Red Wire (Positive)** -> Module **B+**
    *   Battery **Black Wire (Negative)** -> Module **B-**
2.  Connect the **Charger/Boost Module** to the **Arduino Nano**.
    *   Module **OUT+** -> Arduino **VIN**
    *   Module **OUT-** -> Arduino **GND**

### 2. Sensor (MPU-6050) to Arduino Nano (I2C)

| MPU-6050 Pin | Arduino Nano Pin |
| :----------- | :--------------- |
| VCC          | 5V               |
| GND          | GND              |
| SCL          | A5               |
| SDA          | A4               |
| INT          | D2 (Optional)    |

### 3. Wireless Module (nRF24L01+) to Arduino Nano (SPI)

| nRF24L01+ Pin | Arduino Nano Pin | Notes                               |
| :------------ | :--------------- | :---------------------------------- |
| VCC           | **3.3V**         | **CRITICAL: DO NOT USE 5V**         |
| GND           | GND              |                                     |
| CSN           | D10              | SPI Chip Select                     |
| CE            | D9               | Chip Enable                         |
| MOSI          | D11              | Master Out, Slave In                |
| MISO          | D12              | Master In, Slave Out                |
| SCK           | D13              | SPI Clock                           |
| IRQ           | (Unconnected)    | Interrupt Request (not used here)   |

## Software & Code

You will need to program the Arduino Nano using the Arduino IDE. The following libraries will be essential:

*   **`Wire.h`**: For I2C communication with the MPU-6050. (Comes standard with Arduino IDE)
*   **`RF24.h`**: A popular library for controlling the nRF24L01+. You will need to install this via the Arduino Library Manager.
*   **MPU-6050 Library**: A library like `Adafruit_MPU6050` or `MPU6050_tockn` will simplify reading data from the sensor. Install this via the Arduino Library Manager.

A basic program flow would be:
1.  Initialize the MPU-6050 and nRF24L01+.
2.  In the main loop, read the accelerometer and gyroscope data from the MPU-6050.
3.  Transmit the data using the nRF24L01+.

The receiver unit will have a similar program to receive the data and print it to the Serial Monitor.

## Final Assembly

Once you have tested the circuit on a breadboard, you can solder the components together for a more permanent and compact solution. Design and 3D print a custom enclosure or use a small project box to house the electronics, making it a self-contained, wearable unit.
