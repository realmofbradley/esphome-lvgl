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

## Required settings

Set these in the `substitutions:` block of `esphome.yaml`. The build stops
with an error if any is missing.

- `ha_base_url`: your Home Assistant address, for example
  `http://192.168.1.100:8123` (no trailing slash). The display downloads
  weather backgrounds and album artwork from it.
- `weather_entity`: the Home Assistant weather entity shown on the ambient
  face, for example `weather.forecast_home`.
- `bathroom_temperature_entity`: the Home Assistant temperature sensor shown
  in the Bathroom climate pill, for example `sensor.bathroom_temperature`.
- `bathroom_humidity_entity`: the Home Assistant humidity sensor shown in the
  Bathroom climate pill, for example `sensor.bathroom_humidity`.
- `bathroom_light_entity`: the Bathroom light controlled by the Lights
  tile. Both `light.*` and `switch.*` entities are supported, for example
  `light.bathroom` or `switch.top_bathroom_main_lights`.
- `bathroom_fan_entity`: the Bathroom fan controlled by the Fan tile.
  Both `fan.*` and `switch.*` entities are supported, for example
  `fan.bathroom` or `switch.bathroom_fan`.
- `bathroom_heat_entity`: the climate entity shown on the dedicated
  heating controller, for example `climate.bathroom_heater`. It is safe for
  this entity to be unavailable while the heater is offline.

The Night tile needs no helper or substitution. It exposes a stateless **Night**
event entity under the display in Home Assistant. Use its `pressed` event as
the trigger for an automation.

## Weather backgrounds

The background images are **not** compiled into the firmware: at 480x480 they
are too large to fit alongside everything else in flash. The display
downloads them at runtime from Home Assistant instead, so you must copy them
there yourself:

1. Copy every `.jpg` from [assets/backgrounds/](assets/backgrounds/) into
   `/config/www/guition/backgrounds/` on your Home Assistant host (create
   the folders if needed). Keep the filenames unchanged.
2. If `www` did not exist before, restart Home Assistant so it starts serving
   it.
3. Check by opening
   `http://<your-ha-address>:8123/local/guition/backgrounds/clear-day.jpg`
   in a browser.

Files in `www` are served at `/local/` without login, so don't put anything
private in that folder. If the images are missing, the weather face still
works but shows no background.

## Media setup

After the device connects to Home Assistant, set its **Media Player** text
configuration entity to the full entity ID to control (for example,
`media_player.bathroom`). The choice is stored on the display and changing it
reboots once so the native API can negotiate the new dynamic subscriptions.

Now Playing replaces only the ambient weather face while playback is active;
the Bathroom Controls and Music Presets pages remain in the existing swipe
order. Paused artwork stays visible, while idle/off returns to weather after a
30-second grace period.


### Music presets

The Music page has four runtime-configurable Music Assistant presets, a
**Daily Briefing** trigger tile, and a fixed **Stop Music** tile. In the
device's Home Assistant **Configuration** entities, set **Preset 1 Name**,
**Preset 1 Icon**, and **Preset 1 URI**, then repeat for presets 2 through 4.
Values are stored on the display and survive reboots; a firmware rebuild is
not required.

The Daily Briefing tile exposes a stateless **Daily Briefing** event entity
under the display in Home Assistant, matching the Bathroom Night tile. Use
its `pressed` event as the trigger for an automation; the tile does not latch
any UI state.

A preset sends its URI unchanged to `music_assistant.play_media` for the
configured **Media Player**, with `media_type: playlist` and
`enqueue: replace`. The stop tile sends `media_player.media_stop` to the
same player.

Runtime icons are intentionally limited to glyphs compiled into the firmware.
Supported names are `mdi:music`, `mdi:playlist-music`,
`mdi:weather-sunset-up`, `mdi:heart`, `mdi:music-note`,
`mdi:headphones`, `mdi:coffee`, and `mdi:weather-night`. Matching is
case-insensitive and ignores surrounding whitespace. Any other value falls
back to `mdi:music`.

Multi-speaker controls use the same `sensor.speaker_group` data contract as
`jtenniswood/esphome-media-player`: its `data` attribute contains
`ids|names|volumes|manufacturers`. If that helper sensor is absent, the panel
falls back to main-player volume control.
