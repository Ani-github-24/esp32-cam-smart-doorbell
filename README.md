# ESP32-CAM Smart Doorbell System

## Overview
A fully self-hosted smart doorbell system built using **ESP32-CAM**, **ESPHome**, **Home Assistant**, and **Frigate**. The system features real-time person detection, voice alerts, and live camera casting—all running locally without any cloud dependency.

## Why?
Commercial smart doorbells often rely on mandatory cloud subscriptions and raise privacy concerns. This project serves as a privacy-focused, locally processed alternative designed to run efficiently on constrained hardware.

## System Architecture
![Architecture](diagrams/system_architecture.png)

## Features
* **Live Streaming:** Real-time MJPEG streaming via ESP32-CAM.
* **Local AI Detection:** Person detection using Frigate NVR (no external APIs).
* **Event Pipeline:** High-speed MQTT-based communication.
* **Voice Alerts:** Intelligent announcements via Google Home Mini.
* **Smart Casting:** Automatic live camera feed on TV with an auto-resume feature.
* **Touch Input:** Capacitive touch-based doorbell trigger.
* **100% Local:** Entirely self-hosted on a Linux server.

## Tech Stack
* **Hardware:** ESP32-CAM
* **Firmware:** ESPHome
* **Orchestration:** Home Assistant (Docker)
* **NVR/Vision:** Frigate + go2rtc
* **Protocol:** MQTT
* **Media:** Google Cast Integration
* **OS:** Ubuntu Server

## Repository Structure
* `/esphome`: YAML configurations for the ESP32-CAM.
* `/frigate`: Configuration files (optimized for low-power CPUs).
* `/home-assistant`: Automations and dashboard configuration.
* `/diagrams`: Architecture and wiring diagrams.
* `/docs`: Setup guides, tuning tips, and known issues.

## Automations

### Person Detection Flow
* **Logic:** ESP32-CAM frames → Frigate analysis → If person detected → Binary sensor state changes to `detected` → **Home Assistant Trigger**.
* **Action:** Google Home Mini announces: *"Person detected."* (Includes a cooldown timeout).

### Doorbell Touch Flow
* **Logic:** Capacitive touch trigger on ESP32 → Binary sensor state changes to `on` → **Home Assistant Trigger**.
* **Actions:**
    1.  Cast live camera feed to the Google Cast TV.
    2.  Play doorbell chime and announce: *"Someone is at the door."*
    3.  Send a mobile notification via the Home Assistant app.
    4.  Wait 20 seconds, then stop casting.

## Performance & Constraints
* **ESP32 FPS Limits:** Due to hardware and thermal constraints, the camera is limited to **5 FPS at VGA resolution** with a quality value of **10**. This ensures stability for affordable hardware.
* **CPU-based Detection:** Since this runs on an older PC without a hardware accelerator (TPU/GPU), detection takes ~500ms. Resolution in Frigate has been lowered to maintain responsiveness on standard CPU architecture.

## Security & Privacy
* **Zero Cloud:** No data leaves the local network.
* **Secret Management:** Sensitive credentials are excluded from the repository.
* **Local Processing:** All video analysis is performed on-premises.

## Future Improvements
* **Signal Strength:** Add a dedicated external antenna to the ESP32 for better reliability.
* **Night Vision:** Implement IR LEDs for low-light visibility.
* **Field of View:** Upgrade to a wide-angle lens module.

## Author
**Anirudha R**
