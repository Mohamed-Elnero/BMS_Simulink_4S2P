# 🔋 High-Fidelity Lithium-Ion Battery Pack (4S2P) Simulation & BMS Digital Twin

![MATLAB Simulation](https://img.shields.io/badge/MATLAB/Simulink-R2024b%2B-orange.svg)
![Protocol](https://img.shields.io/badge/Telemetry-MQTT-blue.svg)
![BMS Application](https://img.shields.io/badge/Interface-Unity%203D-lightblue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

A comprehensive, high-fidelity **Digital Twin framework** for a **4S2P Configuration Lithium-Ion Battery Pack**. This project couples an electro-thermal physical system simulation implemented in **MATLAB/Simulink (Simscape)** with a live visual application. The embedded **Battery Management System (BMS)** actively monitors safety thresholds, manages cell balancing, and performs state estimation, while synchronously serializing and streaming multi-cell telemetry via **MQTT** to a centralized data **Terminal** (`mqtt-dashboard.com`) for remote visualization.

---

## 📸 System Architecture & Visuals

### 1. High-Fidelity Simulink Multiphysics Model
*Comprehensive top-level simulation workflow incorporating Simscape Battery packs, thermal sensing networks, and the BMS control system layer.*
```html
<p align="center">
  <img src="[https://raw.githubusercontent.com/Mohamed-Elnero/BMS_Simulink_4S2P/main/docs/simulink_model.png](https://raw.githubusercontent.com/Mohamed-Elnero/BMS_Simulink_4S2P/main/docs/simulink_model.png)" width="85%" alt="Simulink Model Architecture">
</p>
