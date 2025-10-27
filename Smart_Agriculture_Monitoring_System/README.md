# Smart Agriculture Monitoring System

This IoT-based project monitors and controls agricultural field conditions in real-time using sensors connected to an ESP32 board and Blynk IoT platform.

## Features
- Monitor soil moisture, humidity, temperature, light, and rain conditions.
- Control irrigation pump automatically or manually through Blynk app.
- Real-time data visualization on a web/mobile dashboard.
- Wi-Fi-based connectivity using ESP32.

## Components Used
- ESP32 board
- DHT11 (Temperature and Humidity Sensor)
- Soil Moisture Sensor
- Rain Sensor
- Light Sensor (LDR)
- Relay Module
- DC Water Pump

## Setup Instructions
1. Open `smart_agriculture.ino` in Arduino IDE.
2. Install required libraries: `WiFi.h`, `BlynkSimpleEsp32.h`, `DHT.h`.
3. Replace your Wi-Fi credentials and Blynk Auth Token in the code.
4. Upload the sketch to ESP32.
5. Use the Blynk app to monitor and control your field.

---
Developed by **Lokesh Perumandla**
