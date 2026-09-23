# ESP32-Recon-Unit-C2
## 📡 Tactical RF Reconnaissance Unit & C2 Station

A modular, distributed **Signal Intelligence (SIGINT)** and **Wireless Intrusion Detection System (WIDS)** platform built for field auditing and physical cyber-security assessments.

The architecture decouples raw radio-frequency (RF) acquisition from heavy analytics: an **ESP32-S3 Edge Node** captures 802.11, BLE, and Sub-GHz signals, streaming high-speed telemetry over UART to a **Python-based Command & Control (C2) Station** for real-time visualization, OSINT enrichment, and forensically sound reporting.

---

## 🏗️ System Architecture

* **Tier 1: Physical Acquisition & Injection Layer (RF & Transceivers Layer)**
  * **Components:** ESP32-S3 native radios (2.4 GHz Wi-Fi + BLE) and CC1101 Sub-GHz transceiver via SPI.
  * **Responsibility:** Passive capture of in-air frames, register-level CSI matrix extraction, BLE advertisement frame scanning, and 433 MHz signal transmission/reception.

* **Tier 2: Processing & Task Allocation Layer (FreeRTOS Core Layer)**
  * **Core 0 (RF Engine):** Exclusively dedicated to strict real-time tasks, capturing and injecting radio frequency packets without interruption.
  * **Core 1 (System & Control):** State machine management, 5-axis joystick input processing, dumping `.pcap` packet captures to the MicroSD card, and UI orchestration.
  * **IPC (Inter-Core Communication):** FreeRTOS Queues and Ring Buffers to transfer radio events seamlessly without losing packets during high-throughput bursts.

* **Tier 3: Local Presentation & Interaction Layer (GUI & Forensics Layer - LVGL)**
  * **Local Rendering:** C++ compiled LVGL graphics engine rendering directly onto the TFT display over a shared SPI bus.
  * **Forensic Storage Management:** FatFS file system for MicroSD card read/write operations (saving WPA2/WPA3 handshakes, CC1101 RAW signal dumps, and captive portal credentials).
  * **Tactical Menus:** On-device panels for target selection, executing Wi-Fi/BLE/Sub-GHz security audits, and monitoring battery and system metrics.

---

## 🚀 Key Features

### 📡 Edge Node Capabilities (ESP32-S3 + COTS Hardware) *(Some examples)*
* **Dual-Core FreeRTOS Architecture:** Core 0 dedicated to high-frequency RF interception; Core 1 handling C2 telemetry streaming, SD logging, and local TFT rendering.
* **802.11 SIGINT & WIDS:** Native promiscuous mode for capturing management frames, handshake extraction (WPA2/WPA3 PMKID), Probe Request auditing, and Deauth attack detection.
* **BLE Ecosystem Profiler:** Passive scanning of Bluetooth Low Energy advertisement payloads (Apple Continuity, Google Fast Pair, iBeacons) and GATT service exposure analysis.
* **Sub-GHz Transceiver (CC1101):** 433 MHz signal capture, raw spectrum visualization, and signal strength auditing for industrial/IoT wireless links.
* **Local Forensics:** Automatic dumping of captured packets to onboard MicroSD in standard `.pcap` format.

### 💻 Command & Control (C2) Station (Python + PyQt6) *(Some examples)*
* **Real-time Waterfall & Spectra:** High-speed, non-blocking telemetry rendering using `PyQtGraph`.
* **Automated OSINT Enrichment:** Automatic MAC-to-OUI vendor resolution against offline industrial and corporate hardware databases.
* **Multi-thread USB Engine:** Asynchronous, thread-safe ingestion thread ensuring zero GUI lag during heavy packet bursts.
* **Report Generator:** One-click PDF generation containing session logs, detected threats, and RF landscape metrics.

---

## 🧰 Hardware Specification (COTS Prototype)

The hardware is built using Commercial Off-The-Shelf (COTS) modules for maximum field reliability and rapid deployment:

| Component | Specification / Model | Interface |
| :--- | :--- | :--- |
| **Microcontroller** | ESP32-S3 DevKitC-1 (N16R8 - 16MB Flash / 8MB PSRAM) | Native USB / UART |
| **Display** | 1.8" / 1.44" TFT Display (ST7735 or ILI9341 driver) | SPI |
| **RF Transceiver** | CC1101 Sub-GHz Module (433 MHz with SMA Antenna) | SPI |
| **Storage** | MicroSD Card Adapter Module | SPI |
| **Power Management** | 18650 Battery Shield V3 / TP4056 + Boost Converter | 5V / 3.3V Rails |
| **Inputs** | 5-Axis Navigation Joystick | GPIO Digital Inputs |

---

## 📄 License & Disclaimer

This project is developed strictly for educational purposes in controlled environments. Unauthorized interception or perturbation of wireless communication networks is illegal.
