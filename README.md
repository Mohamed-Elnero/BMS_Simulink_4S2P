# 🔋 High-Fidelity Lithium-Ion Battery Pack (4S2P) Simulation & BMS Telemetry

![MATLAB Simulation](https://img.shields.io/badge/MATLAB/Simulink-R2024b%2B-orange.svg)
![Protocol](https://img.shields.io/badge/Telemetry-MQTT-blue.svg)
![Accessibility](https://img.shields.io/badge/Data-Terminal-brightgreen.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

A comprehensive simulation framework for a **4S2P Configuration Lithium-Ion Battery Pack**. This project couples an electro-thermal physical system simulation implemented in **MATLAB/Simulink (Simscape)** with an embedded **Battery Management System (BMS)**. 

The system features an automated telemetry sub-system that serializes real-time metrics (Voltage, Current, Temperature, SOC, SOH) into JSON format and publishes them to the public MQTT broker `mqtt-dashboard.com`. This configuration transforms the broker into a universal **Data Terminal**, allowing external applications, remote users, or 3D environments (such as Unity) to subscribe, access, and visualize the live battery data from anywhere.

---

## 📸 System Architecture & Visuals

### 1. High-Fidelity Simulink Multiphysics Model
*Comprehensive top-level simulation workflow incorporating Simscape Battery packs, thermal sensing networks, and the BMS control system layer.*

![](https://raw.githubusercontent.com/Mohamed-Elnero/BMS_Simulink_4S2P/main/docs/simulink_model.png)

### 2. Live Telemetry & MQTT Data Terminal
*Real-time data streaming setup, where the MQTT broker acts as a terminal for external subscribers (Unity, Custom Dashboards, etc.) to monitor battery performance.*

![](https://raw.githubusercontent.com/Mohamed-Elnero/BMS_Simulink_4S2P/main/docs/unity_dashboard.png)

---

## ⚡ Battery Pack Specifications & Topology
The modeled battery pack is constructed from discrete lithium-ion cells with the following technical metrics:
* **Cell Topology:** 4 Series, 2 Parallel (4S2P) — Total of 8 individual cells monitored independently.
* **Single Cell Capacity:** 2.65 Ah
* **Total Pack Capacity:** 5.30 Ah
* **Cell Nominal Voltage:** 3.60 V
* **Max Pack Voltage (Fully Charged):** 16.80 V (4.2V x 4)
* **Min Pack Voltage (Fully Depleted):** 10.80 V (2.7V x 4)

---

## 🛡️ BMS Operating Safety Limits & Thresholds
To guarantee battery safety, optimized life cycles, and mitigate thermal issues, the embedded BMS logic actively enforces strict functional limits:

### 1. Voltage Boundaries
| Parameter | Threshold Value | BMS Protective Action / Description |
| :--- | :--- | :--- |
| **Over-Voltage Fault** | 16.80 V | **Critical:** Triggers immediate charging cutoff to prevent cell swelling. |
| **Over-Voltage Warning** | 16.60 V | **Alert:** Indicates near-full state; initiates charge current derating. |
| **Under-Voltage Warning** | 11.60 V | **Alert:** Low battery user warning; limits heavy load draw. |
| **Under-Voltage Fault** | 10.80 V | **Critical:** Triggers immediate load disconnection to protect cell chemistry. |

### 2. State of Charge (SOC) Window
* **Over-SOC Fault:** 100% (Isolates charger input to prevent overcharging stress)
* **Over-SOC Warning:** 95% (Signals the charging controller to taper down current)
* **Under-SOC Warning:** 20% (Triggers energy-saving mode alert / warning)
* **Under-SOC Fault:** 5% (Deep discharge cutoff to protect cell internal chemistry)

### 3. Current & Temperature Thresholds
* **Max Continuous Discharge Current:** 10.60 A (2C continuous load limit)
* **Max Peak Discharge Current:** 15.90 A (3C instantaneous acceleration limit)
* **Max Fast Charge Current:** 5.30 A (1C safe charging limit)
* **Discharge High-Temperature Fault:** 60.0°C (System shutdown to avoid Thermal Runaway)
* **Discharge High-Temperature Warning:** 45.0°C (Activates cooling configurations & initiates current derating)
* **Low-Temperature Charging Limit:** 0.0°C (Prohibits charging below freezing to prevent Lithium Plating)

---

## 🚀 Key Implemented Features

1. **Dynamic Drive Cycle Loading:** Integrated standard drive cycle profiles to test the system under highly dynamic, real-world current profiles.
2. **Passive Cell Balancing:** Built-in hardware-in-simulation cell balancing circuits to actively equalize State of Charge variations across series-connected cell strings.
3. **Advanced State Estimation:** Equipped with an Extended Kalman Filter (EKF) block for robust, noise-resilient online State of Charge (SoC) and internal resistance ($R_0$) approximation.
4. **Thermal Intelligence & Emulation:** Implemented physical node tracking mimicking an analog temperature sensor network across critical cells to capture degradation and localized heat indices.
5. **IoT Telemetry Gateway:** Embedded a data serialization subsystem packaging live metrics into clean JSON payloads, transmitted over low-latency MQTT protocols to the public broker `mqtt-dashboard.com` to serve as a remote data terminal.

---

## 📊 Core Architecture Technical Breakdown

| Subsystem Component | Technical Method / Tools | Primary Role in Simulation |
| :--- | :--- | :--- |
| **Cell Pack Modeling** | Simscape Battery / `+BatteriesLASTNOTLEAST` | Defines physical cell electro-thermal dynamics and capacity lookups. |
| **State Estimation (SoC)** | Extended Kalman Filter (EKF) | Provides continuous, noise-resilient online state tracking. |
| **Thermal Tracking** | Analog Sensor Network Emulation | Aggregates localized heat indices for BMS protection limits. |
| **IoT Telemetry Terminal** | MATLAB TCP/IP & MQTT Client Engine | Transforms variables into JSON arrays for live external data access. |

---

## 📁 Repository File Directory Guide

* `+BatteriesLASTNOTLEAST/` : Custom Simscape Components and `.ssc` files that model the core cell physical structures and assembly parameters.
* `BatteriesLASTNOTLEAST.slx` : The main executable Simulink workspace containing the primary system blocks and logic connections.
* `BatteriesLASTNOTLEAST_param.m` : Initializer script containing system constants, thermal resistances, and capacity lookup vectors.
* `model_one_upgrade2.slx` & `model_one_upgrade3.slx` : Iterative upgrades tracking intermediate BMS development phases.

---

## 🛠️ How to Run & Initialize

1. **Clone the Repository cleanly to your workspace:**
   ```bash
   git clone [https://github.com/Mohamed-Elnero/BMS_Simulink_4S2P.git](https://github.com/Mohamed-Elnero/BMS_Simulink_4S2P.git)
