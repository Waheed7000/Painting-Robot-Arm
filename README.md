# Painting Robot Arm

## 🚀 Overview
This project implements a **robotic arm capable of painting a wall**, controlled via a **handheld motion controller**.

The system translates natural hand movements into smooth robotic motion using IMU-based sensing and real-time control.

---

## 🎥 Project Demonstration

[Watch the Wireless Control Unit Demonstration](https://youtu.be/xwx8KO3fbKQ)
___

## 🧠 System Concept

The user holds a controller and tilts it:
- ↔️ Left / Right → controls **Yaw (horizontal movement)**
- ↕️ Forward / Backward → controls **Pitch (vertical movement)**

These movements are converted into normalized commands and sent wirelessly to the robot.

---

## 🏗️ System Architecture

```
Hand Motion
    ↓
MPU6050 (IMU)
    ↓
Filtering + Sensor Fusion
    ↓
Roll / Pitch Angles
    ↓
Mapping → Normalized Commands [-1, 1]
    ↓
Wireless Communication (Wi-Fi)
    ↓
Robot Unit
    ↓
Stepper Motor Control
    ↓
Robot Arm Movement
```

---

## 🎮 Control Unit

### Hardware
- ESP32
- MPU6050 (Accelerometer + Gyroscope)
- 7.4V Li-ion Battery
- Buck Converter (3.3V)

### Features Implemented
- Raw IMU data acquisition
- Sensor scaling (g, deg/s)
- Roll & Pitch computation
- Gyroscope calibration (startup)
- Complementary filter (sensor fusion)
- Accelerometer low-pass filtering
- Dead zone filtering
- Reference pose (zero orientation)
- I2C communication validation
- Modular firmware architecture

---

## 📐 Orientation Processing

### Complementary Filter
Combines:
- Gyroscope → smooth motion
- Accelerometer → long-term stability

```
angle = 0.98 * (gyro) + 0.02 * (accel)
```

---

## 🎯 Control Mapping

Angles are converted into normalized velocity commands:

```
cmd = angle / MAX_CONTROL_ANGLE
```

Example:
```
15° / 30° = 0.5
```

### Command Range
| Value | Meaning |
|------|--------|
| +1.0 | Max forward speed |
|  0.0 | Stop |
| -1.0 | Max reverse speed |

---

## 🤖 Robot Unit

### Joints
1. Linear axis → Forward / Backward (distance from wall)
2. Yaw axis → Left / Right
3. Pitch axis → Up / Down

### Control Logic
```
speed = command × MAX_SPEED
```

Then:
```
speed → steps/sec → motor pulses
```

---

## 🧮 Advanced Feature (Planned)

### Automatic Distance Compensation

Goal:
Maintain a constant distance from the wall while moving.

Approach:
- End-effector moves on a spherical surface
- Use Forward Kinematics:

```
depth = f(yaw, pitch, link_length)
```

- Adjust forward/backward axis automatically

---

## 📡 Communication (Planned)

- ESP32 ↔ ESP32 (Wi-Fi)
- Encrypted channel
- Data format:
```
cmd_yaw, cmd_pitch
```

---

## 📏 Sensor Considerations

| Sensor | Status |
|-------|--------|
| Ultrasonic | ❌ Not recommended |
| ToF (VL53L0X) | ✅ Recommended |

Best approach:
```
Model-based control + Sensor correction
```

---

## 📊 Project Status

### ✅ Completed
- IMU system
- Filtering & calibration
- Command mapping
- Clean modular code structure

### 🔄 In Progress
- Power electronics
- Mechanical assembly

### ⏳ Upcoming
- Wireless communication
- Robot motor control
- Forward kinematics
- Distance compensation

---

## 🧠 Key Engineering Decisions

- Velocity-based control (not position)
- Separation of system layers
- Complementary filter over Kalman
- Normalized command interface [-1, 1]

---

## 📦 Code Structure

```
control_unit/
├── control_unit.ino
├── config.h
├── imu.h
├── imu.cpp
```

---

## 🧪 Example Output

```
Roll: 12.5   Pitch: -8.2   |   YawCmd: 0.42   PitchCmd: -0.27
```

---

## 🎯 Summary

```
Controller:
hand tilt → roll/pitch → normalized commands

Robot:
commands → speed → steps → motors

Advanced:
forward kinematics → automatic depth control
```

---

## 👨‍💻 Author Notes

This project focuses on:
- Real-time control
- Clean architecture
- Scalable design
- Strong engineering fundamentals

---

## 📌 Future Improvements
- PID / smoothing on robot side
- UI feedback system
- Safety limits and constraints
- Advanced trajectory planning

---

## ⭐ Final Goal

A smooth, intuitive, and intelligent painting robot that:
- Feels natural to control
- Maintains stable surface distance
- Demonstrates strong system design

---
