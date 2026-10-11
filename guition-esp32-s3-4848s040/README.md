# Guition ESP32-S3 4848S040 (4.0")

4-inch touch LCD panel with ESP32-S3, running ESPHome and LVGL for home automation and sensor displays.

![Guition ESP32-S3 4848S040](../images/guition-esp32-s3-4848s040.jpg)

## Configuration

- **Template**: [esphome.yaml](esphome.yaml) — use the **contents of this file as your ESPHome config** in the dashboard or CLI (create or edit your device config so it matches this file).

## Where to Buy

- **Panel**: [AliExpress](https://s.click.aliexpress.com/e/_c3sIhvBv) (~£16)

## Stand

- **Desktop stand** (3D printable): [MakerWorld](https://makerworld.com/en/models/2327976-touch-screen-desktop-stand-for-guition-4848s040#profileId-2543111)

## Folder Structure

```
guition-esp32-s3-4848s040/
├── addon/          # Time, network, backlight
├── assets/         # Fonts and icons
├── device/         # device.yaml, sensors.yaml, lvgl.yaml
├── theme/          # Button and UI styling
├── esphome.yaml    # ESPHome config template
```

Customize for your setup by editing the YAML files under `device/`, `addon/`, and `theme/`. See the [main README](../README.md) for full quick start and ESPHome setup.

## Media setup

After the device connects to Home Assistant, set its **Media Player** text
configuration entity to the full entity ID to control (for example,
`media_player.bathroom`). The choice is stored on the display and changing it
reboots once so the native API can negotiate the new dynamic subscriptions.

Now Playing replaces only the ambient weather face while playback is active;
the Bathroom Controls and Music Presets pages remain in the existing swipe
order. Paused artwork stays visible, while idle/off returns to weather after a
30-second grace period.

Multi-speaker controls use the same `sensor.speaker_group` data contract as
`jtenniswood/esphome-media-player`: its `data` attribute contains
`ids|names|volumes|manufacturers`. If that helper sensor is absent, the panel
falls back to main-player volume control.
