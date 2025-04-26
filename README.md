# Laser Rail Precision Scanner (Mark I)

---

## Mission

Build a **3D-printed laser table** with **high-precision rail movement** to **scan physical objects** in real-time and **visualize them** using **Godot Engine**.

---

## Core System Design

| Module                                        | Purpose                                                         |
| :-------------------------------------------- | :-------------------------------------------------------------- |
| **Laser Module**                              | Fires laser beams at set intervals to measure distances         |
| **Rail Sled**                                 | Moves laser across X and Y axes with ultra-fine control         |
| **Measurement Microcontrollers** (3x Pico Ws) | Each handles rail movement, laser firing, or data communication |
| **Master C# Control Program**                 | Orchestrates all Pico units, aggregates data                    |
| **Godot Visualizer**                          | Renders live 3D point cloud from incoming scan data             |

---

## Movement System

- **String Drive**:

  - Stepper motors pull the sled using strong, low-stretch string (e.g., braided fishing line or kevlar).
  - Bearings provide smooth, frictionless glide along rails.

- **Stepper Motor Specs**:

  - 1.8° Step Angle (200 steps/rev)
  - 1/16th Microstepping (3200 steps/rev)
  - Pulley ~20mm Diameter (customizable)

- **Precision Goal**:
  - ~**0.01mm movement** per microstep achievable.

---

## Electronics Layout

| Component                            | Notes                                        |
| :----------------------------------- | :------------------------------------------- |
| **3x Raspberry Pi Pico W**           | Microcontroller brains                       |
| **Stepper Motors (NEMA 17 or mini)** | Rail movement drivers                        |
| **Motor Drivers (A4988/DRV8825)**    | Power interface between Pico and motors      |
| **Laser Distance Sensor**            | Measures object surface distance             |
| **Wiring (USB or Serial)**           | Fast low-latency communication to desktop PC |

---

## Software Stack

| Layer                        | Tech                                                           |
| :--------------------------- | :------------------------------------------------------------- |
| **Microcontroller Firmware** | C++ (bare metal control of motors, laser, sensors)             |
| **Desktop Commander**        | C# app handling communication and command sequencing           |
| **Visualizer**               | Godot Engine project reading live data and building object map |

---

## System Flow

```plaintext
[ C# Commander App ]
       ↓
(Send move command)
       ↓
[ Pico Rail Controller ]
(Move sled 0.01mm)
       ↓
(Send fire command)
       ↓
[ Pico Laser Controller ]
(Fire laser, read distance)
       ↓
(Send back distance data)
       ↓
[ C# Commander collects node ]
       ↓
(Send node to Godot)
       ↓
[ Godot creates point cloud in real time ]
```
