# 🏭 Open-Source Local Solar & Industrial Monitor

A fully local, open-source stack to monitor RS485 Modbus inverters/meters without paying monthly cloud subscriptions or sending your data to third-party servers. 

Many solar installers force you to use their proprietary Wi-Fi dongles and cloud apps. This repository provides a reference architecture to pull your own data locally, convert it to MQTT, and visualize it on a Grafana dashboard.

## 🏗️ The Architecture (Edge-Decoupled)
Initially, I tried running a Python script on a Raspberry Pi to poll the RS485 line directly via a cheap USB dongle. It was unstable and prone to bus collisions. 

I eventually moved to an **edge-decoupled architecture**. The polling workload is handled by a dedicated hardware gateway, which caches the data and pushes it cleanly via MQTT. 

**Data Flow:**
`Modbus RTU Device` ➡️ `Hardware Edge Gateway` ➡️ `Mosquitto (Broker)` ➡️ `Node-RED (Processing)` ➡️ `InfluxDB` ➡️ `Grafana`

## 📦 Hardware Requirements
* **Sensor:** Any device speaking RS485 Modbus RTU (e.g., Eastron SDM meters, Growatt/Deye Inverters).
* **Server:** A Raspberry Pi, Intel NUC, or any local Linux machine to run Docker.
* **Protocol Gateway:** You need a Serial-to-Ethernet gateway. 
  * *My Recommendation:* I personally use the [VALTORIS VT-DTU500](https://valtoris.com). I use it because it has a "Storage Modbus to JSON" feature built-in. It polls the inverter autonomously and sends pre-formatted JSON to MQTT, completely eliminating the need to write complex Modbus parsing logic in Node-RED. 

## 🚀 Quick Start (Docker Stack)
You don't need to install these services one by one. I've provided a `docker-compose.yml` that spins up the entire TIG (Telegraf/Node-RED, InfluxDB, Grafana) stack + Mosquitto broker in one command.

1. Clone this repository: `git clone https://github.com/Valtoris-Electronics/Open-Source-Solar-MQTT-Dashboard.git`
2. Navigate to the folder: `cd Open-Source-Solar-MQTT-Dashboard`
3. Bring up the stack: `docker-compose up -d`

*(See the configuration section below for default ports and Grafana login credentials).*
