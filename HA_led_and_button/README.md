| Supported Targets | ESP32-H2 | ESP32-C6 | ESP32-C5 |
| ----------------- | -------- | -------- | -------- |

# Combined Light and Switch Zigbee Example

This example combines both a Home Automation On/Off Light and On/Off Switch on a single Zigbee end device, allowing it to act as both a controllable light and a physical switch.

## Attribution and Original Sources

Based on two official ESP-Zigbee SDK examples from Espressif Systems:

1. **HA_on_off_light**: [esp-zigbee-sdk/examples/esp_zigbee_HA_sample/HA_on_off_light](https://github.com/espressif/esp-zigbee-sdk/tree/main/examples/esp_zigbee_HA_sample/HA_on_off_light)
2. **HA_on_off_switch**: [esp-zigbee-sdk/examples/esp_zigbee_HA_sample/HA_on_off_switch](https://github.com/espressif/esp-zigbee-sdk/tree/main/examples/esp_zigbee_HA_sample/HA_on_off_switch)

Original code © 2021-2022 Espressif Systems (Shanghai) CO LTD, Licensed under CC0-1.0

## Key Changes from Original Examples

1. **Simplified LED Control**: Replaced complex `light_driver` with direct GPIO control on GPIO15
2. **Combined Architecture**: Single board runs both light and switch endpoints:
   - **Endpoint 10**: On/Off Light (receives commands)
   - **Endpoint 1**: On/Off Switch (sends commands)
3. **Dual Functionality**: Device responds to external commands AND can send commands via button press
4. **Retained Switch Driver**: Uses original button handling with debouncing

## Hardware Required

* One 802.15.4 enabled development board (ESP32-H2, ESP32-C6, or ESP32-C5)
* LED connected to GPIO15 (built-in LED on some boards)
* Boot button on development board (GPIO configured via menuconfig)

## How It Works

The device starts as a Zigbee End Device with two endpoints:
- **Light endpoint (10)**: Receives ZCL On/Off commands and controls LED on GPIO15
- **Switch endpoint (1)**: Sends `on_off_toggle` commands when boot button is pressed

For the switch functionality to control the light, both endpoints need to be bound together in the Zigbee network.

## Configure the project

Set the correct chip target: `idf.py set-target TARGET`

The button GPIO is configurable via menuconfig under `Component config` → `Board Support Package`.

## Build and Flash

Erase NVRAM (recommended): `idf.py -p PORT erase-flash`

Build and flash: `idf.py build flash monitor`

## Application Functions

### Network Joining
```
I (432) ESP_ZB_ON_OFF_LIGHT&SWITCH: Initialize Zigbee stack
I (442) ESP_ZB_ON_OFF_LIGHT&SWITCH: Device started up in factory-reset mode
I (3382) ESP_ZB_ON_OFF_LIGHT&SWITCH: Joined network successfully
```

### Light Control (Receiving Commands)
```
I (7162) ESP_ZB_ON_OFF_LIGHT&SWITCH: Received message: endpoint(10), cluster(0x6), attribute(0x0), data size(1)
I (7162) ESP_ZB_ON_OFF_LIGHT&SWITCH: Light sets to On
```

### Switch Control (Sending Commands)
```
I (11743) ESP_ZB_ON_OFF_LIGHT&SWITCH: Send 'on_off toggle' command
```
