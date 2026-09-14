# BCI-Based Smart Wheelchair — Custom PCB

## Overview

This repository contains the design and development files for a **custom 2-layer PCB developed for a Brain-Computer Interface (BCI)-based Smart Wheelchair**.

The PCB was designed to provide a centralized and modular hardware interface for the wheelchair's **ESP32 controller, BCI subsystem, sensors, communication modules, display, and motor-control interface**.

The primary objective of the board is to replace the complex wiring used during prototyping with a structured PCB-based architecture that improves **system integration, reliability, maintainability, and expandability**.

---

## System Architecture

The custom PCB acts as the primary hardware interface between the controller and the various subsystems of the wheelchair.

```text
                    BCI / EEG System
                           │
                           ▼
                     Arduino Nano
                           │
                           │ UART
                           ▼
                    ┌──────────────┐
                    │    ESP32     │
                    │ Main Control │
                    └──────┬───────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   Health Sensors     Safety Sensors     Communication
        │                  │                  │
   MAX30102/DHT11      MPU6050/IR/US        GPS / IoT
        │                  │
        └──────────────────┼─────────────────┘
                           │
                           ▼
                   Motor Control Interface
                           │
                           ▼
                     Motor Driver
                           │
                           ▼
                    Wheelchair Motors
```

---

## PCB Objectives

The board was developed with the following objectives:

* Centralize the connections between the ESP32 and external modules.
* Provide dedicated interfaces for the wheelchair's sensors and peripherals.
* Reduce prototype wiring complexity.
* Provide organized power and ground distribution.
* Maintain a modular architecture for easier debugging and replacement.
* Provide an interface to the external high-current motor driver.
* Allow future hardware modifications and system expansion.

---

## Main Hardware Interfaces

The PCB provides interfaces for the following modules:

| Module             | Function                           | Interface                 |
| ------------------ | ---------------------------------- | ------------------------- |
| ESP32              | Main controller                    | GPIO / I²C / UART / ADC   |
| Arduino Nano       | BCI subsystem interface            | UART                      |
| EXG Pill           | EEG / eye-blink signal acquisition | BCI interface             |
| MAX30102           | Heart rate & SpO₂                  | I²C                       |
| MPU6050            | Motion & tilt monitoring           | I²C                       |
| DHT11              | Temperature & humidity             | Digital GPIO              |
| GPS Module         | Location tracking                  | UART                      |
| Ultrasonic Sensors | Obstacle detection                 | GPIO                      |
| IR Sensors         | Line following / detection         | GPIO                      |
| MQ Sensor          | Gas/environment monitoring         | ADC                       |
| OLED               | Local status display               | I²C                       |
| Motor Driver       | DC motor control                   | Digital control interface |

---

## BCI Control

The wheelchair incorporates an EEG/eye-blink-based control mechanism using an **EXG Pill** and **Arduino Nano**.

Detected blink patterns are mapped to movement commands:

| Blink Pattern | Command |
| ------------- | ------- |
| 1 Blink       | STOP    |
| 2 Blinks      | FORWARD |
| 3 Blinks      | LEFT    |
| 4 Blinks      | RIGHT   |

The processed control information is transferred to the ESP32, which coordinates the command with the wheelchair's safety and sensing systems.

The BCI subsystem is kept modular so that the signal acquisition and processing hardware can be modified independently from the main controller PCB.

---

## Sensor Integration

### Health Monitoring

The board provides interfaces for health-monitoring modules including the **MAX30102** for heart-rate and SpO₂ measurement and the **DHT11** for temperature and humidity monitoring.

### Obstacle Detection

Multiple ultrasonic sensor interfaces are provided for monitoring the environment around the wheelchair.

The planned sensor arrangement includes:

* Front
* Left
* Right
* Rear

Distance information is processed by the ESP32 and can be used by the control system for obstacle-awareness and safety functions.

### Motion Monitoring

The **MPU6050** provides accelerometer and gyroscope data for detecting changes in wheelchair orientation and tilt.

### Line Following

Dedicated IR sensor interfaces are provided for line-following functionality and other detection requirements.

### GPS

A UART interface is provided for connecting a GPS module for location tracking and caregiver monitoring.

---

## ESP32 Pin Allocation

The current design uses the following ESP32 GPIO allocation:

| Function         |    GPIO |
| ---------------- | ------: |
| I²C SDA          |      21 |
| I²C SCL          |      22 |
| GPS UART         | 16 / 17 |
| MQ Sensor        |      34 |
| DHT11            |       4 |
| IR Sensor 1      |       5 |
| IR Sensor 2      |      33 |
| IR Sensor 3      |      26 |
| Ultrasonic Front | 12 / 13 |
| Ultrasonic Left  | 14 / 15 |
| Ultrasonic Right | 18 / 19 |
| Ultrasonic Rear  | 23 / 25 |
| Nano Interface   | 32 / 27 |

The pin assignment may be revised in future hardware or firmware versions.

---

## PCB Design

The PCB was designed as a **2-layer board using KiCad**.

The design process involved:

1. **System planning**
   Identifying the required interfaces, power requirements, and communication buses.

2. **Schematic development**
   Creating the electrical schematic and defining the connections between the ESP32 and external modules.

3. **Component and footprint selection**
   Selecting appropriate components, connectors, and footprints based on electrical and mechanical requirements.

4. **PCB layout**
   Arranging components according to functionality while considering accessibility and routing requirements.

5. **Routing**
   Routing signal and power traces while maintaining an organized PCB layout.

6. **Design verification**
   Performing electrical and design-rule checks before preparing the manufacturing outputs.

7. **Manufacturing preparation**
   Generating the required fabrication files for PCB manufacturing.

---

## Modular Hardware Design

A key design consideration was maintaining **modularity**.

External sensors and modules are connected through dedicated headers rather than being permanently integrated into the PCB.

This allows:

* Individual modules to be replaced easily.
* Faster hardware debugging.
* Easier testing during development.
* Simplified wiring.
* Future sensor upgrades.
* Hardware expansion without completely redesigning the system.

The high-current motor-driving stage is implemented using an external motor-driver module, while the custom PCB provides the required control interface.

---

## Power Distribution

The PCB includes organized power and ground distribution for the connected low-power modules.

Decoupling components are incorporated near relevant power connections to improve supply stability and reduce noise.

The motor power path is kept separate from the low-power controller and sensor circuitry because wheelchair motors can draw significantly higher current than the logic subsystem.

For a production-grade design, additional protection and power-management features would be required.

---

## Repository Structure

---

## Design Files

This repository contains the relevant hardware development files, including:

* KiCad schematic
* KiCad PCB layout
* Gerber fabrication files
* Drill files
* Bill of Materials
* PCB design documentation
* PCB renders and images

---

## Future Improvements

Potential improvements for future revisions include:

* Integrated power-management circuitry
* Reverse-polarity protection
* Over-voltage and over-current protection
* Dedicated battery monitoring
* Improved EMI/EMC design
* Hardware-level emergency-stop circuitry
* Improved motor-driver isolation
* USB-C programming/debugging interface
* Integrated ESP32 module
* Reduced PCB form factor
* Dedicated production-grade connectors
* Improved enclosure and mechanical mounting

---

## Technical Skills Demonstrated

This project involved practical work in:

* PCB schematic design
* 2-layer PCB layout
* KiCad
* Component and footprint selection
* GPIO planning
* I²C and UART interfacing
* Sensor integration
* ESP32 hardware integration
* Power distribution
* Connector design
* PCB routing
* Design Rule Checking (DRC)
* Electrical Rule Checking (ERC)
* Gerber generation
* Hardware debugging
* Embedded-system integration

---

## Disclaimer

This project is an **engineering prototype developed for educational and research purposes**.

It is not a certified medical device or safety-certified wheelchair control system. Deployment in a real assistive mobility application would require comprehensive electrical, mechanical, software, functional-safety, EMC, reliability, and regulatory validation.

---

## Project Focus

**PCB Design · Embedded Systems · BCI · Robotics · Assistive Technology · ESP32 · IoT · KiCad**

---

## Author

**Jishin Bijumon George**

Electronics & Communication Engineering
Focused on **PCB Design, Embedded Systems, IoT, and Hardware Product Development**
