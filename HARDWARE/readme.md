# ESP32-Based 4WD RC Car
### Hardware Design & Control System for ORB-SLAM Project

## 1. Project Overview

This repository contains the hardware design and control system implementation of a four-wheel-drive (4WD) Remote Control (RC) Car developed as part of our team's ORB-SLAM project.

The RC car is built around an ESP32 microcontroller, which manages four DC motors through two L298N motor drivers. The vehicle is controlled wirelessly using a PlayStation 4 (PS4) controller via Bluetooth communication, implemented with the Bluepad32 library.

The primary objective of this platform is to provide a mobile robotic vehicle capable of maneuvering in different directions, serving as a hardware platform for further development and integration with ORB-SLAM (Oriented FAST and Rotated BRIEF Simultaneous Localization and Mapping).

## 2. Hardware Components

The following components are used in the RC car development:

| No. | Component | Quantity | Description |
|---|---|---:|---|
| 1 | RC Car Chassis | 1 | Mechanical frame of the vehicle |
| 2 | DC Motors | 4 | Provide propulsion for the four wheels |
| 3 | Wheels | 4 | Vehicle movement and traction |
| 4 | L298N Motor Driver | 2 | Control motor direction and speed |
| 5 | ESP32 Development Board | 1 | Main microcontroller and Bluetooth interface |
| 6 | 18650 Li-ion Battery (3.7V) | 2 | Main power source |
| 7 | PlayStation 4 Controller | 1 | Wireless vehicle controller |

## 3. System Architecture

The system consists of three primary subsystems:

**A. Control Subsystem**
- PS4 Controller provides user input.
- Bluetooth communication is handled by the ESP32 using the Bluepad32 library.
- The ESP32 processes joystick inputs and translates them into motor control commands.

**B. Motor Drive Subsystem**
- Two L298N motor drivers control four DC motors.
- Each driver handles two motors.
- PWM signals regulate motor speed.
- Digital GPIO signals determine motor rotation direction.

**C. Power Subsystem**
- Two 18650 Li-ion batteries supply electrical power to the system.
- The power distribution must accommodate the ESP32, motor drivers, and DC motors.

### System Workflow

PS4 Controller → Bluetooth Communication → ESP32 → Motor Control Logic → L298N Drivers → Four DC Motors → Vehicle Movement

## 4. ESP32 Pin Configuration

The following GPIO configuration is implemented in the Arduino program.

### Motor Driver 1 – Front Motors

| Motor | PWM / EN | IN1 | IN2 |
|---|---:|---:|---:|
| Right Front (RF) | GPIO 13 | GPIO 14 | GPIO 27 |
| Left Front (LF) | GPIO 33 | GPIO 25 | GPIO 26 |

### Motor Driver 2 – Rear Motors

| Motor | PWM / EN | IN1 | IN2 |
|---|---:|---:|---:|
| Left Rear (LR) | GPIO 32 | GPIO 19 | GPIO 18 |
| Right Rear (RR) | GPIO 21 | GPIO 22 | GPIO 23 |

### PWM Configuration

| Parameter | Value |
|---|---|
| PWM Frequency | 5,000 Hz |
| PWM Resolution | 8-bit |
| PWM Duty Cycle | 0–255 |
| Joystick Deadzone | 40 |
| Control Update Delay | 20 ms |

The 8-bit PWM resolution provides 256 duty-cycle levels (0–255) for motor speed regulation.

## 5. Control Algorithm

The vehicle uses an **Arcade Drive Control Algorithm**, which combines throttle and steering inputs to independently regulate the left and right motor groups.

### Joystick Mapping

| Controller Input | Function |
|---|---|
| Left Stick – Y Axis | Forward / Backward |
| Right Stick – X Axis | Left / Right Steering |

### Motor Mixing

The joystick inputs are normalized into a range of -255 to +255.

The motor speed commands are calculated using:

`Left Speed = Throttle + Steering`

`Right Speed = Throttle - Steering`

Both values are constrained to the range of -255 to +255.

The resulting commands are distributed to the four motors:

- Left side: Left Front + Left Rear.
- Right side: Right Front + Right Rear.

This approach enables the vehicle to perform:
- Forward movement.
- Backward movement.
- Left and right turns.
- In-place rotation (differential steering).
- Variable-speed movement.

### Motor Direction Control

Each motor is controlled using two digital direction pins and one PWM enable pin.

| IN1 | IN2 | Motor State |
|---|---|---|
| HIGH | LOW | Forward |
| LOW | HIGH | Reverse |
| LOW | LOW | Stop |

The program also includes individual motor direction inversion settings to compensate for differences in motor mounting orientation.

## 6. Software & Libraries

The firmware is developed using the Arduino framework for ESP32.

**Required software and libraries:**
- Arduino IDE
- ESP32 Arduino Core
- Bluepad32 Library

**Main source file:**

`rc_car_4motor.ino`

The firmware includes:
- Bluetooth controller pairing and connection management.
- PS4 joystick input processing.
- Joystick deadzone filtering.
- PWM-based motor speed regulation.
- Differential steering using arcade mixing.
- Individual motor direction configuration.
- Automatic motor stop when the controller disconnects.

## 7. Safety & Power Considerations

The vehicle uses two 18650 Li-ion batteries rated at 3.7V nominal voltage each.

The battery configuration (series or parallel) must be verified before operation because it determines the system's supply voltage.

Additional considerations:
- Ensure that the motor supply voltage is compatible with the DC motor specifications.
- Provide an appropriate regulated power supply for the ESP32.
- Verify the current rating of the L298N drivers against the motor requirements.
- Ensure proper wiring and common ground between the ESP32 and motor drivers.
- Use appropriate battery protection and charging circuits.

## 8. Project Integration: ORB-SLAM

This RC car is developed as the mobile hardware platform for our team's ORB-SLAM implementation.

The motor control system provides the mobility required for the vehicle to navigate its environment. Further integration with a camera and a processing platform will enable visual data acquisition and simultaneous localization and mapping.

The intended system development includes:
- Mobile robotic platform development.
- Visual data acquisition.
- Feature detection and visual tracking.
- Simultaneous Localization and Mapping (SLAM).
- Real-time environment mapping and localization.

*Note: Camera integration, processing hardware, and ORB-SLAM implementation are separate development components and are not included in the current motor-control firmware.*

## 9. Repository Structure

```text
RC-Car-ORB-SLAM/
│
├── README.md
│
├── firmware/
│   └── rc_car_4motor.ino
│
├── hardware/
│   ├── circuit-diagram/
│   └── hardware-photos/
│
└── documentation/
    └── system-architecture/
```

## 10. Development Status

| Subsystem | Status |
|---|---|
| RC Chassis Assembly | Hardware implementation |
| Four-Motor Drive System | Implemented |
| ESP32 Motor Control | Implemented |
| Bluetooth Communication | Implemented |
| PS4 Controller Integration | Implemented |
| Differential Steering | Implemented in firmware |
| ORB-SLAM Integration | Separate development stage |

---

**Project Focus:** Embedded Systems · Mobile Robotics · Wireless Control · Computer Vision · SLAM

**Platform:** ESP32 · Arduino · Bluepad32 · L298N
