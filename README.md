# 📟 Home Assistant E-Paper Dashboard with Proxmox Integration

This project displays key smart home and server stats on a **7.5" e-paper screen** using an **ESP32-C3 board** running [ESPHome](https://esphome.io). It’s designed for a clean, always-on visual dashboard of:

- 🏠 Home temperature readings  
- 📡 Service uptime (via Uptime Kuma)  
- ☁️ Weather overview  
- 💾 Proxmox LXC container resource usage (CPU, memory, disk)

---

## 🛠️ Hardware

Although the configuration is compatible with generic **ESP32-C3 + Waveshare 7.5" ePaper (B/W, model `7.50inv2`)**, this project uses the **Seeed Studio [XIAO 7.5" ePaper Panel](https://www.seeedstudio.com/XIAO-ePaper-7.5-inch-p-5747.html)** – a compact, all-in-one development kit that integrates:

- **XIAO ESP32-C3**
- **7.5" ePaper display**
- Pre-wired SPI and power handling

This makes it ideal for a neat, professional installation without custom wiring.

> 🎨 **Note:** The panel is currently in its default white enclosure, but I plan to paint it **matte black** to better match the rest of my setup.

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
2. Flash `dashboard.yaml` to your Seeed Studio **XIAO 7.5" ePaper Panel** using ESPHome
3. Update `wifi`, `api`, and any `sensor` or `text_sensor` entity IDs to match your Home Assistant setup
4. Mount and enjoy your ambient dashboard!
