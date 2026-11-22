# Home Assistant blueprints

Blueprints I use in my own Home Assistant and share with the community.

| Blueprint | Description |
| --- | --- |
| [🧹 Vacuum Maintenance & Off-Peak Charging (Roborock)](#-vacuum-maintenance--off-peak-charging-roborock) | Consumables, errors, dock water levels and solar/price-driven off-peak charging for Roborock vacuums |

---

## 🧹 Vacuum Maintenance & Off-Peak Charging (Roborock)

[![Open your Home Assistant instance and show the blueprint import dialog with this blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Ffbloemhof%2Fha-blueprints%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fvacuum-maintenance.yaml)

One blueprint to monitor a Roborock vacuum (and optionally its dock) through the
official [Roborock integration](https://www.home-assistant.io/integrations/roborock/).
Pick the vacuum and dock devices; all entities are detected automatically.

### Features

- 🧹 **Consumable parts**: main brush, side brush, filter and sensors on the vacuum,
  strainer and cleaning brush on the dock. The notification has a **Reset** button
  that presses the matching `button.reset_*_consumable`.
- 📅 **Hasn't cleaned in a while**: daily check against the last clean.
- 🚨 **Errors**: vacuum and dock error sensors with readable names, which you can
  override or translate. **Locate** and **Resume** buttons when the robot is trapped
  or its wheels are off the ground. A notification is only sent or cleared when the
  state changes.
- 💧 **Dock water levels**: clean water tank empty, dirty water tank full.
- ⚡ **Off-peak charging window**: sets `time.*_off_peak_start` / `_end` around
  tomorrow's solar peak (for example from Solcast), or otherwise on the cheapest
  consecutive block of tomorrow's [Nord Pool](https://github.com/custom-components/nordpool)
  prices. Works with both hourly and 15-minute prices.
- 🔋 **Battery boost**: if a round ends early on a low battery (an aging battery that
  can't finish a full round), off-peak charging is paused so the dock charges right
  away. It is switched back on once the battery is full.
- 🌐 Every notification text can be changed, so you can translate the blueprint
  completely.

Each feature can be switched off on its own.

### Requirements

- Home Assistant **2024.10** or newer
- The Roborock integration
- The [Companion app](https://companion.home-assistant.io/) for notifications
  (`notify.mobile_app_*`)
- Optional: a solar forecast sensor with a timestamp of tomorrow's peak, and/or a
  Nord Pool (custom integration) price sensor with a `raw_tomorrow` attribute

### Good to know

- **Entity detection** works by entity-ID suffix (for example `_main_brush_time_left`,
  `_dirty_box_full`). English and Dutch entity names are supported. If you renamed
  an entity ID, or your model doesn't have the entity, that feature does nothing.
  There are no errors.
- **Dock**: on auto-empty/auto-wash models the dock is a separate device in Home
  Assistant. Select it under *🔌 Dock* for water levels and dock consumables.
- **Errors and water levels** are checked every 5 minutes. A push message is only
  sent when a state changes, so the 500-per-day push limit of the Companion app is
  not at risk.
- **Off-peak window**: if neither a solar peak nor tomorrow's prices are available,
  the current window is left unchanged. Select the price sensor in the blueprint so
  the window is recalculated as soon as tomorrow's prices arrive.
- **Battery boost** turns `switch.*_off_peak_charging` back on at the "full"
  threshold. It does not do this if you switched it off yourself in Home Assistant.
- **iOS**: notifications and buttons work. Sticky/persistent notifications and
  custom icons are Android-only.
- **Multiple vacuums**: create one automation per vacuum. Notification tags and
  buttons are unique per robot.

### Changelog

- **2026-11-22**: first public release. Errors and water levels now only notify on a
  state change, and the off-peak window supports 15-minute prices. There is a new
  price sensor input, and the window is kept when no prices are known. Notification
  buttons are only handled by the instance that owns the entities, and tags are
  unique per vacuum.

## License

[MIT](LICENSE)
