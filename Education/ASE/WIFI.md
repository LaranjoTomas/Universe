---
tags:
  - "#wifi"
  - "#wireless"
  - "#Networks"
---
# 4 ideas of what WIFI is

Wifi is a wireless network technology, which uses radio waves to provide wireless high-speed Internet access. 
Provides convenient ways to connect devices within a limited area.
Operates in 2.4 Ghz and 5 Ghz bands.
# What WiFi version is supported by ESP32-c3?

R: The ESP32-C3’s radio supports 2.4 GHz Wi-Fi under the IEEE 802.11b/g/n standard (i.e. “Wi-Fi 4”), with HT20 and HT40 operation (MCS0–7) for up to 150 Mb/s data rates
# Which WiFi networking APIs are provided with ESP/IDF?

ESP-NOW, ESP-WiFi-MESH, SmartConfig, esp_wifi (Wi-Fi Driver API, .e.g. esp_wifi.h), Wi-Fi Easy Connect (DPP), Wi-Fi Aware (NAN).  
# What are the restrictions on using WiFi and BLE simultaneously in an ESP32-C3 SoC?

On the ESP32-c3, Wifi and BLE share a single 2.4Ghz RF (analog hardware) front-end and so cannot  literaly transmit or receive at the very same instant. 