# esphome-common

Reusable ESPHome common configurations and hardware definitions for Home Assistant integrations.

## Structure

```
common/                     # Reusable config fragments
  common.yaml               # Standard bundle (WiFi, API, OTA, time, sensors, buttons)
  common_eink.yaml          # Stripped-down bundle for e-ink displays (minimal heap usage)
  config-api.yaml           # HA API with encryption
  config-ota.yaml           # OTA updates
  config-time.yaml          # SNTP time sync (requires ${timezone} substitution)
  config-wifi.yaml          # WiFi with AP fallback and captive portal
  config-wifi-minimal.yaml  # WiFi only, no fallback
  sensor/                   # Connection status, uptime, WiFi signal, WiFi info
  button/                   # Restart and safe mode buttons

hardware/
  esp32/
    crowpanel-4.2.yaml      # Elecrow CrowPanel ESP32-S3 4.2" e-paper HMI
    waveshare-eink.yaml     # Waveshare e-paper ESP32 board (7.5" V2)
    sonoff-basic-r4.yaml    # Sonoff Basic R4 (ESP32-C3)
  esp8266/
    sonoff-basic-r2.yaml    # Sonoff Basic R2
    sonoff-4ch-pro.yaml     # Sonoff 4CH Pro
```

## Required substitutions

All device configs must define these substitutions:

| Variable | Description | Example |
|---|---|---|
| `node_name` | ESPHome device name (lowercase, no spaces) | `crowpanel` |
| `friendly_node_name` | Human-readable name | `CrowPanel` |
| `timezone` | IANA timezone string | `Europe/Stockholm` |

## Usage

### Local development

Clone this repo and reference files with relative `!include` paths:

```yaml
# your-device.yaml
substitutions:
  node_name: my-device
  friendly_node_name: My Device
  timezone: Europe/Stockholm

packages:
  common: !include ../esphome-common/common/common.yaml
  hardware: !include ../esphome-common/hardware/esp32/crowpanel-4.2.yaml
```

### Via GitHub (ESPHome packages)

```yaml
substitutions:
  node_name: my-device
  friendly_node_name: My Device
  timezone: Europe/Stockholm

packages:
  esphome_common:
    url: https://github.com/UglyBob79/esphome-common
    ref: master
    files:
      - common/common.yaml
      - hardware/esp32/crowpanel-4.2.yaml
```

## secrets.yaml

The following secrets are required:

```yaml
wifi_ssid: "your-ssid"
wifi_password: "your-password"
wifi_fallback_password: "fallback-password"  # only needed for common.yaml (not minimal)
enc_key: "your-base64-encryption-key"        # generate with: openssl rand -base64 32
```
