# ha-blueprint-dtcontrol
Advanced differential fan control for Home Assistant featuring automated efficiency checks and thermal guarding

This Home Assistant blueprint provides an advanced logic to control a fan (or heat transfer system) based on the temperature difference between two rooms. It is designed for energy-efficient heating or cooling by moving air only when it's effective.

## Features

* **Differential Control:** Starts the fan only when Room A is significantly warmer than Room B.
* **Hysteresis:** Separate thresholds for starting and stopping to prevent rapid switching.
* **Source Protection:** Set a minimum temperature for the source room to avoid cooling it down too much.
* **Efficiency Check (Optional):** Monitors if the target room's temperature actually rises. If the gain is too low (e.g., due to heat loss), the fan stops to save power.
* **Timeout & Cooldown:** Prevents endless fan operation and enforces a break before the next attempt.
* **Schedule & Master Switch:** Integrated support for time-based operation and a manual bypass.

## Prerequisites

1.  **Temperature Sensors:** Two sensors (Source and Target).
2.  **Switch:** A smart plug or switch controlling the fan.
3.  **Storage Helper:** You MUST create an `input_number` helper (Helper -> Number) to store the temperature for the efficiency check.
    * Range: 0 to 50 (or your typical temp range).
    * Step: 0.1
4.  **Master Switch:** An `input_boolean` to enable the automation.
5.  **Schedule:** A `schedule` helper to define allowed operating hours.

## Installation

You can easily import this blueprint by clicking the button below:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fscrap-code%2Fha-blueprint-dtcontrol%2Fmain%2Fdtcontrol.yaml)

### Manual Installation
1. Copy the URL of the `dtcontrol.yaml` file.
2. Go to your Home Assistant instance.
3. Navigate to **Settings** > **Automations & Scenes** > **Blueprints**.
4. Click **Import Blueprint** and paste the URL.

## Configuration Guide

* **dT Start:** The temperature difference required to start the fan (e.g., 4K).
* **dT Stop:** The difference at which the fan stops (e.g., 1.5K).
* **Check Interval:** How often the automation evaluates the temperature gain (default: 15 min).
* **Min Gain:** If Room B doesn't heat up by this amount during one interval, the fan enters cooldown.

## License
GNU GPL v3
