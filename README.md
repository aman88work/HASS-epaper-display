# ESPHome + Home Assistant E-Paper Dashboard

A simple, low-power dashboard using ESPHome and Home Assistant, displayed on a 7.5" e-paper device. It shows weather, temperatures, service status, and Proxmox container stats, cycling through splash, home, and Proxmox screens.

---

## Purpose

Provide key home and system data at a glance—without needing to unlock or tap anything. It’s always visible, silent, and easy to customize.

---

## Key Features

- 3 auto-rotating screens:  
  1. Splash image  
  2. Weather, indoors/outdoors, service health  
  3. Proxmox container resource usage (CPU, memory, swap)  
- Clean layout with Material Design icons  
- Very low energy use—no backlight, always readable  
- Runs on a Seeed Studio **XIAO 7.5" ePaper Panel**, fully integrated and easy to install

---

## Hardware Overview

The project uses the [Seeed Studio XIAO 7.5" ePaper Panel](https://www.seeedstudio.com/XIAO-7-5-ePaper-Panel-p-6416.html?srsltid=AfmBOoo7yNJdh6ocDCPpBVsW7EUONfYskGAK5dhxONjg4-Wjx3BBmSTa), which includes:

- Built-in **XIAO ESP32-C3**, Wi-Fi capable  
- 7.5" 800×480 e-paper display  
- USB-C port, boot/reset buttons, and optional battery  
- Tight integration with ESPHome and Home Assistant via their wiki guide :contentReference[oaicite:0]{index=0}

Note: I plan to paint the panel matte black to blend with my setup.

---

## Setup Instructions

### 1. Install ESPHome in Home Assistant

- Go to **Settings → Add-ons → Add-on Store**
- Find ESPHome, click **Install**, then **Start**
- If it’s missing, you may need a supported Home Assistant installation. For Home Assistant Container, use ESPHome Device Builder via Docker :contentReference[oaicite:1]{index=1}.

---

### 2. Add a New Device in ESPHome

- Select **New Device**, choose a name (e.g., `epaper_dashboard`)
- Pick ESP32-C3 as the board, then click **Edit**

---

### 3. Flash the Dashboard

- Replace the auto-generated YAML with `dashboard.yaml` from this repo
- Update Wi-Fi and Home Assistant API credentials
- Customize any sensor IDs to match your Home Assistant setup

#### Flashing Options:
- **USB (initial setup)**: Click **Install → Manual Download**, then use the web uploader to flash the panel
- **OTA (subsequent updates)**: After initial setup, updates can be pushed wirelessly if `ota:` and `api:` are properly configured

For full ESPHome setup and examples, see the official [Seeed wiki](https://wiki.seeedstudio.com/xiao_075inch_epaper_panel_esphome/) .

---

## Customization

This dashboard reflects my setup—but you can adapt it:

- Add different sensors (e.g., energy usage, security status)  
- Re-order or combine slides  
- Change icons, fonts, or layout elements

---

## Repo Contents

- `dashboard.yaml` – ESPHome configuration  
- `fonts/` – Font files including Material Design icons  
- `image/` – Boot splash image (optional)

---

## License

Feel free to use, modify, and build on this—no restrictions.

---
