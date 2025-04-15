![toolxox com-enhanced-XArSH6gzj7Wt82PRCZFK9Tu4dV0EBy](https://github.com/user-attachments/assets/915adf5a-b5bf-4cd8-ab84-ca5586013527) 
# 🔊 Two-Way Intercom Device Firmware

### Developed by **Ashwanth**

---

## 📦 Overview

This repository contains the firmware for a **Two-Way Intercom Device** designed for communication over a local network (LAN). The firmware includes essential components to manage intercom operations, such as LAN initialization, server handshake, call status handling, and basic logging functionalities.

The device is structured to emulate a landline-style intercom system, suitable for private networks, residential communities, industrial campuses, or building security infrastructure.

---

## ⚙️ Features

- 🔐 **Device Registration & Authentication**  
  Simulated server-based handshake with primary and backup address support.

- 🌐 **LAN Initialization**  
  Assigns a static IP, MAC address, subnet, and gateway for controlled network behavior.

- ☎️ **Call Management**  
  Simulates receiving and handling of incoming intercom calls with speaker and mic control.

- 🗂️ **Call Logging**  
  Captures and stores call metadata including number, timestamp, and call direction (incoming/outgoing).

- 📞 **Landline Number Mapping**  
  Configurable landline numbers for each device and emergency routing.

- 📡 **Server Communication**  
  Connects to predefined server endpoints for registration and status updates.

---

## 🔧 Configuration

```cpp
// Device Network Settings
IPAddress deviceIp(192, 168, 10, 55);
IPAddress gateway(192, 168, 10, 1);
IPAddress subnet(255, 255, 255, 0);

// Server Addresses
char serverPrimary[] = "intercom.central.fake-server.local";
char serverBackup[] = "192.168.10.99";

// Landline Numbers
const char* lineOne = "044-22334455";
const char* lineTwo = "044-99887766";
const char* emergencyLine = "044-00001111";
