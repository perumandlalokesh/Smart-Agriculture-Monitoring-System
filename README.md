# 🌾 Smart Agriculture Monitoring System

### 🧠 Project Overview
The **Smart Agriculture Monitoring System** is an IoT-based solution designed to automate irrigation and monitor environmental parameters in real-time.  
It helps farmers optimize water usage, improve crop health, and make data-driven agricultural decisions using affordable hardware.

---

### ⚙️ Features
- Real-time monitoring of:
  - 🌡️ Air Temperature & Humidity (DHT11)
  - 🌱 Soil Moisture
  - 🌧️ Rain Detection
  - 💡 Light Intensity
- Automatic and manual irrigation control
- Remote monitoring via **Blynk IoT App**
- LED indicators for pump, rain, and automation status
- Battery-powered backup for continuous operation

---

### 🧩 Hardware Components
| Component | Function |
|------------|-----------|
| ESP32 / ESP8266 | Wi-Fi enabled microcontroller |
| DHT11 | Measures air temperature and humidity |
| Soil Moisture Sensor | Detects soil water level |
| Rain Sensor | Detects rainfall presence |
| LDR Sensor | Measures light intensity |
| Relay Module | Controls water pump |
| DC Pump | Used for irrigation |
| Battery | Backup power source |

---

### 🖥️ Software Requirements
- Arduino IDE (v1.8+)
- Blynk Library
- DHT Sensor Library
- Wi-Fi connection for ESP32/ESP8266

---

### 🧾 Partial Implementation (Draft Code)
This repository contains a **partial implementation (approx. 40%)** of the project, focusing on:
- Sensor integration and calibration  
- Blynk IoT connectivity  
- Auto/manual pump control logic  
- LED status updates for automation feedback

You can find the main source file here:  
📄 **`smart_agriculture.ino`**

---

### 🚀 How to Run
1. Open `smart_agriculture.ino` in **Arduino IDE**.  
2. Update the following fields in your code:
   ```cpp
   char ssid[] = "YourWiFiName";
   char pass[] = "YourPassword";
   #define BLYNK_AUTH_TOKEN "YourBlynkAuthToken"
