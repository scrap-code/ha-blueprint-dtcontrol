# Differential Temperature Control Pro

A Home Assistant blueprint that controls a fan based on the temperature difference between two rooms. Instead of reacting to absolute temperatures, it activates when one space is meaningfully warmer than another. This is useful for passive heat transfer, e.g. moving solar-gained warmth from a south-facing room into a cooler one.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fscrap-code%2Fha-blueprint-dtcontrol%2Fmain%2Fdtcontrol.yaml)

## How it works

The automation runs as a persistent loop. On each cycle it checks whether start conditions are met, runs the fan, monitors stop conditions every minute, then waits for the next relevant state change before checking again.

**Start conditions (all must be true):**
- Temperature differential A−B exceeds `dT Start`
- Room A is above `Minimum Source Temperature`
- Room B is below the target temperature
- Master switch is on and within schedule

**Stop conditions (any will stop the fan):**
- Differential drops below `dT Stop`
- Room B reaches target temperature
- Maximum runtime (`Timeout`) exceeded
- Efficiency check fails - Room B hasn't risen by `Min Gain` within a `Check Interval`

After each fan cycle a `Cooldown` period prevents immediate restart.

## Requirements

- Two temperature sensors (source and target room)
- A smart switch or plug for the fan
- An `input_boolean` helper - master on/off switch
- A `schedule` helper - defines permitted operating hours
- *(Optional)* An `input_number` helper (0–40 °C, step 0.1) - for a dynamic target temperature that can change over time

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| Source Temperature Sensor | - | Sensor in the warmer room (Room A) |
| Target Temperature Sensor | - | Sensor in the room to be heated (Room B) |
| Fan Actuator | - | Switch or smart plug controlling the fan |
| Automation Master Switch | - | `input_boolean` to enable/disable |
| Operation Schedule | - | Schedule helper defining allowed hours |
| Minimum Source Temperature | 18 °C | Room A floor — prevents over-cooling the source |
| Target Temperature Room B (fixed) | 21 °C | Fallback target for Room B |
| Target Temperature Room B (entity) | - | Optional entity override for dynamic targets |
| dT Start | 4 K | Differential required to activate the fan |
| dT Stop | 1.5 K | Differential at which the fan stops (hysteresis) |
| Maximum Runtime | 60 min | Hard timeout per fan cycle |
| Cooldown Period | 30 min | Rest time after each cycle before restart |
| Enable Efficiency Check | false | Halt fan if Room B isn't warming fast enough |
| Minimum Temperature Gain | 0.2 K | Required rise in Room B per check interval |
| Check Interval | 15 min | How often the efficiency baseline is evaluated |

## Companion Card

The [Differential Temperature Card](https://github.com/scrap-code/ha-card-difftemp) is a custom Lovelace card built for this blueprint. It shows a target temperature slider, live room temperatures, fan status with a runtime counter, and a colour-coded delta-T display (green above 3 K, yellow 0-3 K, red when source is cooler than target). Installable via HACS.

## License

GNU GPL v3 — see [LICENSE](LICENSE).
