# 📟 Home Assistant E-Paper Dashboard with Proxmox Integration

This project displays key smart home and server stats on a **7.5" e-paper screen** using an **ESP32-C3 board** running [ESPHome](https://esphome.io). It’s designed for a clean, always-on visual dashboard of:

- 🏠 Home temperature readings  
- 📡 Service uptime (via Uptime Kuma)  
- ☁️ Weather overview  
- 💾 Proxmox LXC container resource usage (CPU, memory, disk)

---

## 🔧 Hardware

- **ESP32-C3 dev board**
- **Waveshare 7.5" e-Paper (B/W)** – model `7.50inv2`
- Powered via USB or 5V regulator

---

## 📋 Features

- Uses **ESPHome** with **Home Assistant API**
- Displays rotate automatically between:
  1. **Splash screen** (photo)
  2. **Home overview** (weather, temps, uptime)
  3. **Proxmox stats** (LXC resource usage)
- Simple and elegant layout using custom fonts and Material Design icons
- No interaction needed – auto-refreshing every 30 seconds

---

## 📦 Home Assistant Integration

Make sure the following are available in Home Assistant:
- Temperature sensors (`sensor.*_temperature`)
- Weather entity (`weather.forecast_home`)
- UptimeKuma sensors (`sensor.uptimekuma_*`)
- Proxmox LXC sensors (e.g. `sensor.lxc_pihole_cpu`, `sensor.lxc_homeassistant_memory`, etc.)

---

## 📁 Repository Contents

- `dashboard.yaml` – Full ESPHome config for the dashboard
- `fonts/` – Font files including Material Design icons
- `image/` – Splash image for boot screen

---

## 🚀 Getting Started

1. Clone this repo
2. Flash `dashboard.yaml` to your ESP32-C3 board via ESPHome
3. Update `wifi`, `api`, and any `sensor` or `text_sensor` entity IDs to match your HA setup
4. Mount and enjoy your ambient dashboard!
