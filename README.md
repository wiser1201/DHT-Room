# ESP8266 DHT11 IoT Monitoring System

This project is an end-to-end IoT project for monitoring temperature and humidity using **ESP8266** microcontroller and **DHT11** temperature-humidity sensor.

The repository contains two submodules: cloud and esp. Cloud corresponds to a backend logic, implemented in **Python**. ESP corresponds to esp firmware, written in **C**, that contains provisioning and application itself.



## System Architecture

The system consists of two independent parts:

1. **ESP8266 firmware**
   - ESP8266-RTOS SDK
   - Handles device web provisioning
   - Reads DHT11 sensor data
   - Sends data to the cloud
   - Manages persistent device state via NVS
   - HTML + js page embedded in flash
2. **Cloud backend**
   - FastAPI for backend logic
   - sqlite3 for databases
   - User authentication and sessions
   - Device management
   - Web interface for viewing sensor data



## Device Workflow

1. On first boot, the device starts in provisioning mode
2. ESP8266 creates a Wi-Fi Access Point
3. User connects to the AP and is redirected to a provisioning page via wildcard DNS
4. User enters Wi-Fi credentials
5. Device attempts to connect to the specified network:
   - On success: credentials are saved, device reboots
   - On failure: user is notified and provisioning continues
6. Device state is stored in NVS and checked on every boot
7. In normal operation mode:
   - Sensor data is read
   - Converted to JSON
   - Sent to the cloud
   - Device enters sleep mode
8. The cloud handles all http requests and databases