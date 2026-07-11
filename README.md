# 🔋 Battery Pack Simulation with Integrated BMS and MQTT Telemetry Terminal

## 📌 Project Overview
This repository contains a high-fidelity battery system simulation model developed in **MATLAB/Simulink (Simscape)**. The project models an 8-cell lithium-ion battery pack configured in a **4S2P topology** integrated with a functional **Battery Management System (BMS)**. 

A core feature of this implementation is the telemetry sub-system: all real-time measurements calculated by the BMS (Voltage, Current, Temperature, SOC, SOH) are serialized into JSON format and continuously published to the public MQTT broker **`mqtt-dashboard.com`**. This configuration transforms the broker into a centralized data **Terminal**, enabling any external application or remote user to access, monitor, and utilize the live battery telemetry from anywhere.

---

## ⚡ Battery Pack Specifications & Topology
The battery pack is built using lithium-ion cells with the following configurations:
* **Cell Topology:** 4 Series, 2 Parallel (4S2P) — Total of 8 Cells
* **Single Cell Capacity:** $2.65 \text{ Ah}$
* **Total Pack Capacity:** $5.30 \text{ Ah}$
* **Cell Nominal Voltage:** $3.60 \text{ V}$
* **Max Pack Voltage (Fully Charged):** $16.80 \text{ V}$ ($4.2\text{V} \times 4$)
* **Min Pack Voltage (Fully Depleted):** $10.80 \text{ V}$ ($2.7\text{V} \times 4$)

---

## 🛡️ BMS Operating Safety Limits (Thresholds)
To guarantee battery safety and longevity, the embedded BMS logic actively monitors and enforces strict thresholds:

### 1. Voltage Limits
| Parameter | Threshold Value | BMS Action / Description |
| :--- | :--- | :--- |
| **Over-Voltage Fault** | `16.80 V` | **Critical:** Triggers immediate charging cutoff to prevent cell swelling. |
| **Over-Voltage Warning** | `16.60 V` | **Alert:** Indicates near-full state; initiates charge current derating. |
| **Under-Voltage Warning** | `11.60 V` | **Alert:** Low battery user warning; limits heavy load draw. |
| **Under-Voltage Fault** | `10.80 V` | **Critical:** Triggers immediate load disconnection to protect cell chemistry. |

### 2. State of Charge (SOC) Window
* **Over-SOC Fault:** `100%` (Isolates charger to prevent overcharging stress)
* **Over-SOC Warning:** `95%` (Signals the charging controller to taper down current)
* **Under-SOC Warning:** `20%` (Triggers energy-saving mode alert)
* **Under-SOC Fault:** `5%` (Deep discharge cutoff to protect cell chemistry)

### 3. Current & Temperature Limits
* **Max Continuous Discharge Current:** `10.60 A` ($2\text{C}$ continuous load limit)
* **Max Peak Discharge Current:** `15.90 A` ($3\text{C}$ instantaneous acceleration limit)
* **Max Fast Charge Current:** `5.30 A` ($1\text{C}$ safe charging limit)
* **Discharge High-Temperature Fault:** `60.0°C` (System shutdown to avoid Thermal Runaway)
* **Discharge High-Temperature Warning:** `45.0°C` (Activates cooling plates & initiates current derating)
* **Low-Temperature Charging Limit:** `0.0°C` (Prohibits charging below freezing to prevent Lithium Plating)

---

## 🚀 Key Features Implemented
1. **Dynamic Drive Cycle Loading:** Integrated a standard drive cycle source to simulate real-world dynamic load currents.
2. **Passive Cell Balancing:** Implemented balancing circuits to normalize state-of-charge disparities across the series-connected cells.
3. **Advanced State Estimation:** Equipped with estimators (such as Extended Kalman Filter / RLS) to approximate internal resistance ($R_0$) and State of Health (SOH).
4. **IoT Telemetry Terminal:** Embedded an automated data serialization sub-system packaging live metrics into clean JSON formats transmitted directly over MQTT protocols to `mqtt-dashboard.com` for easy remote access.

---

## 📁 Repository Structure
* `model_one_upgrade2.slx` - Main Simulink architecture including Simscape blocks and BMS control logic.
* `BatteriesLASTNOTLEAST_param.m` - Comprehensive initialization script containing cell lookup data, thermal resistances, and BMS constants.
