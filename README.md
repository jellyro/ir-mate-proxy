# IR Mate Proxy - ESPHome Package

ESPHome package for the [XIAO Smart IR Mate](https://www.seeedstudio.com/XIAO-Smart-IR-Mate-p-6492.html), a compact Wi-Fi infrared remote control hub based on the Seeed Studio XIAO ESP32-C3.

## Features

- **IR proxy** — IR receiver and transmitter in homeassistant
- **Touch sensor** — single, double, triple, and quad click events
- **Haptic feedback** — instant vibration tick on every touch (toggleable)
- **Status LED** — color-coded device state at a glance:
  - Blinking blue: starting up
  - Off: Wi-Fi connected
  - Solid orange: AP/fallback mode
  - Solid red: Wi-Fi disconnected
- **Reset button** — short press restarts, long press (10s+) factory resets
- **Vibration patterns** — short, medium, long, double, and triple (testable from HA)
- **Captive portal** — Wi-Fi provisioning when fallback AP is active

## Usage

Add the package to your device config:

```yaml
substitutions:
  name: my-ir-proxy
  friendly_name: My IR Proxy

packages:
  jellyro.ir-mate-proxy: github://jellyro/ir-mate-proxy/ir-mate-proxy.yaml@main

esphome:
  name: ${name}
  friendly_name: ${friendly_name}
  name_add_mac_suffix: false

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

ota:
  - platform: esphome
    password: !secret my_ir_proxy__ota_password

api:
  encryption:
    key: !secret my_ir_proxy__encryption_key
```

The package provides Wi-Fi (with AP fallback), captive portal, and all hardware logic. The parent provides the ESPHome name, Wi-Fi credentials, OTA, and API encryption key. Wi-Fi fields merge — the package's `id`, `power_save_mode`, `reboot_timeout`, `on_connect`/`on_disconnect` logging, and `ap` fallback are all pre-configured.

If you omit the `esphome` block in the parent, the package defaults to `name: ir-mate-proxy` with `name_add_mac_suffix: true`, giving each device a unique name automatically.

## Optional substitutions

Override these in your parent config to customize:

| Substitution | Default | Description |
|---|---|---|
| `name` | `my-ir-proxy` | ESPHome device name |
| `friendly_name` | `My IR Proxy` | Home Assistant display name |
| `ir_tx_pin` | `GPIO3` | IR transmitter pin |
| `ir_rx_pin` | `GPIO4` | IR receiver pin |
| `rgb_led_pin` | `GPIO7` | Status LED pin |
| `touch_pin` | `GPIO5` | Touch sensor pin |
| `vibration_pin` | `GPIO6` | Vibration motor pin |
| `reset_button_pin` | `GPIO9` | Reset button pin |

## Home Assistant entities

| Entity | Type | Description |
|---|---|---|
| Touch Action | Event | Fires on single/double/triple/quad click |
| Haptic Feedback | Switch | Enable/disable touch vibration |
| IR Transmitter | Infrared | Forward IR commands to transmit |
| IR Receiver | Infrared | Receive/learn IR commands |
| Status | Binary sensor | API connection status |
| Factory Reset | Switch | Trigger factory reset |
| Restart | Button | Restart device |
| Restart in Safe Mode | Button | Restart in safe mode |
| WiFi Signal | Sensor | RSSI (diagnostic, hidden) |
| Uptime | Sensor | Device uptime (diagnostic, hidden) |
| IP Address | Text sensor | Current IP (diagnostic) |
| Connected SSID | Text sensor | Wi-Fi SSID (diagnostic) |
| Vibrate Short/Medium/Long/Double/Triple | Buttons | Test vibration patterns (hidden) |

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

You are free to use, modify, distribute, and sublicense this software for any purpose, including commercial use, at no cost.

## Disclaimer

**This software is provided "as is", without warranty of any kind, express or implied. Use at your own risk.**

The authors and copyright holders shall not be liable for any claim, damages, or other liability arising from the use of this software. This includes but is not limited to hardware damage, data loss, fire, injury, or any other consequence of using or misusing this software or the hardware it controls.

**No affiliation.** This project is not affiliated with, endorsed by, or sponsored by Seeed Studio, Espressif Systems, ESPHome, Open Home Foundation, or Home Assistant. All product names, trademarks, and registered trademarks are the property of their respective owners and are used for identification purposes only.
