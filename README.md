# HASS ePaper Display — ESPHome ESP32-C3 Smart Home Dashboard

[![Releases](https://img.shields.io/badge/Releases-Download-blue)](https://github.com/aman88work/HASS-epaper-display/releases)

![7.5" e-paper + ESP32-C3](https://www.waveshare.com/w/upload/7/7f/E-Paper-7.5.jpg)

Overview
- This project shows smart home and server stats on a 7.5" e-paper screen.
- It runs on an ESP32-C3 board using ESPHome.
- It pulls data from Home Assistant sensors and local servers.
- The design favors low power and long-term readability on e-paper.

Features
- Persistent display with partial and full refresh support.
- Shows temperature, humidity, door state, energy use, and server load.
- Multiple layout templates for day, night, and compact modes.
- OTA updates via ESPHome or manual flashing.
- Configurable update schedule to save power and reduce wear on the e-paper panel.

Quick links
- Releases and firmware: https://github.com/aman88work/HASS-epaper-display/releases
  - Download the release asset named hass-epaper-display-esp32c3.bin from the Releases page and flash it to your ESP32-C3. Execute the binary by flashing it to the board (see Flashing section).

Why this repo
- Use e-paper to show always-on home data without draining power.
- Use ESPHome so the device integrates with Home Assistant with native sensors.
- Keep the display readable from a distance and friendly to low-light rooms.

Hardware
- 7.5" e-paper display (Waveshare recommended)
- ESP32-C3 dev board or module
- Level shifter for SPI signals (if needed)
- 3.3V power supply with at least 500 mA
- Wires and a small protoboard or adapter

Core pins (example)
- VCC -> 3.3V
- GND -> GND
- DIN (MOSI) -> ESP32-C3 GPIO 7
- CLK (SCK) -> ESP32-C3 GPIO 6
- CS -> ESP32-C3 GPIO 10
- DC -> ESP32-C3 GPIO 9
- RST -> ESP32-C3 GPIO 8
- BUSY -> ESP32-C3 GPIO 5

Wiring varies by panel model. Check your panel's datasheet for exact pins.

Software stack
- ESPHome (YAML based firmware)
- Home Assistant for sensors and automations
- Optional local server metrics (Prometheus, Node Exporter, or simple scripts)

Sample ESPHome YAML
- Use this as a starting point. Adjust pin mapping to match your board and display.

```yaml
esphome:
  name: hass_epaper_display
  platform: ESP32
  board: esp32-c3-devkitm-1

wifi:
  ssid: "YOUR_SSID"
  password: "YOUR_PASS"

logger:

api:
  reboot_timeout: 0s

ota:
  password: "ota_password"

spi:
  clk_pin: 6
  mosi_pin: 7

display:
  - platform: waveshare_epaper
    cs_pin: 10
    dc_pin: 9
    busy_pin: 5
    reset_pin: 8
    model: 7.5in
    update_interval: 300s
    lambda: |-
      it.print(10, 20, id(font_large), "Home");
      it.printf(10, 60, id(font_medium), "Temp: %.1f°C", id(sensor_temp).state);
      it.printf(10, 90, id(font_small), "CPU: %.0f%%", id(server_cpu).state);

font:
  - file: "fonts/Roboto-Regular.ttf"
    id: font_small
    size: 16
  - file: "fonts/Roboto-Regular.ttf"
    id: font_medium
    size: 26
  - file: "fonts/Roboto-Regular.ttf"
    id: font_large
    size: 40

sensor:
  - platform: homeassistant
    id: sensor_temp
    entity_id: sensor.living_room_temperature
  - platform: homeassistant
    id: server_cpu
    entity_id: sensor.server_cpu_usage
```

Layout and templates
- Design the layout to use large text and clear icons.
- Use a small set of fonts for consistency.
- Use high-contrast elements. E-paper renders best in black and white.
- Limit refresh frequency. Each full refresh costs power and time.

Flashing and Releases
- Visit Releases and download the firmware asset.
- Use the release link: https://github.com/aman88work/HASS-epaper-display/releases
- Download the file named hass-epaper-display-esp32c3.bin from the latest release.
- Flash the binary to your ESP32-C3.

Example using esptool.py:
- Install esptool if you do not have it.
  - pip install esptool
- Put your device in bootloader mode.
- Run:
  - esptool.py --chip esp32c3 --port /dev/ttyUSB0 write_flash -z 0x1000 hass-epaper-display-esp32c3.bin

Example using ESPHome:
- Use esphome to build the YAML above.
  - esphome run hass_epaper_display.yaml
- ESPHome will compile and upload the firmware over USB or OTA.

If you prefer OTA:
- Use the ESPHome dashboard.
- Upload the compiled firmware file from the Releases page or your build.
- The device accepts OTA if configured with ota: in the YAML.

Home Assistant integration
- The device exposes sensors through native API.
- Add the device via Integrations -> ESPHome.
- Expose display mode switches and REST endpoints if you want remote control.
- Example automations:
  - Update the display every 10 minutes between 6:00 and 23:00.
  - Switch to a compact mode at night.
  - Show a server alert screen when CPU > 85% or a sensor shows a critical state.

Server metrics
- Pull local server stats via Home Assistant sensors or HTTP sensors.
- Use Node Exporter + Prometheus if you run a monitoring stack.
- Use a small script to expose system stats to Home Assistant if you prefer a lightweight approach.

Power and refresh tips
- Use partial refresh when only a few elements change.
- Use full refresh every few hours to avoid ghosting.
- Set update_interval higher for screens that show few changes.
- Use a stable 3.3V supply to avoid glitches during refresh.

Debugging
- If the display shows garbage:
  - Check wiring for MOSI, CLK, CS, RST, DC, and BUSY.
  - Check that SPI pins match the YAML.
  - Check that the model matches your panel in the display: model field.
- If ESPHome fails to upload:
  - Confirm the board in esphome matches your hardware.
  - Put the board in bootloader mode if USB upload fails.
- If data is missing:
  - Confirm Home Assistant entity IDs.
  - Check entity availability in Developer Tools -> States.

FAQ
- Q: Which e-paper models work?
  - A: The code targets Waveshare 7.5" B/W panels. Other vendors may work with pin and model changes.
- Q: Can I use a different ESP32 board?
  - A: Yes. Change the board value in esphome and pin mapping.
- Q: Will the e-paper burn out?
  - A: E-paper panels wear with heavy full refreshes. Use partial updates and longer intervals.

Customization
- Change fonts to match your wall decor.
- Add a calendar or to-do list from Home Assistant.
- Add icons for doors, lights, and weather.
- Create multiple templates in your YAML and switch them via a Home Assistant input_select.

Contributing
- Fork the repo, make a branch for your feature, and open a pull request.
- Write clear commit messages.
- Add or update hardware notes if you test new panels or boards.
- Provide sample YAML when you add features.

Images and assets
- Use high-contrast icons for the e-paper.
- Include fonts in the repo under a fonts/ folder.
- Use SVG icons for small detail and raster images for photos only.

License
- Use an open license for firmware and assets.
- Choose MIT or Apache 2.0 to allow reuse and improvements.

Credits
- ESPHome for the firmware framework.
- Waveshare for the e-paper panels and datasheets.
- Home Assistant for the integration model.

Releases (again)
- Visit the Releases page to get the ready-made firmware and assets:
  - https://github.com/aman88work/HASS-epaper-display/releases
- Download the hass-epaper-display-esp32c3.bin file and flash it to your ESP32-C3 to run the project.

Screenshots
- Add screenshots of layouts to the repo's /screenshots folder.
- Example:
  - A daily layout with sensors.
  - An alert layout for server issues.
  - A minimal layout for night mode.

License file
- Add a LICENSE file at the repo root.
- Use a permissive license if you want contributions.

Maintainer
- Open issues for bugs and feature requests.
- Use clear titles and steps to reproduce.
- Provide logs and photos for hardware issues.

Icons and emoji
- Use simple emojis in automations and README to make the content scannable:
  - 🔋 for battery or power
  - 🌡️ for temperature
  - 💡 for light or switch
  - 🖥️ for server stats
  - 📶 for Wi-Fi and connectivity

Start now
- Clone the repo.
- Edit the YAML to match your pins and Home Assistant entities.
- Build locally or download the prebuilt firmware from Releases and flash the binary.
- Integrate with Home Assistant and tune the layout to your room.