# HA Calendar Event Start

A reusable Home Assistant automation blueprint that runs actions from calendar
events whose titles contain selected text.

[![Open your Home Assistant instance and import this blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmn22680%2FHA-Calendar-Event-Start%2Fblob%2Fmain%2Fcalendar_text_command.yaml)

## What it does

- Watches any Home Assistant calendar, including Google Calendar.
- Matches calendar titles case-insensitively, either exactly or partially.
- Provides configurable start, end, pause, and resume actions.
- Works with vacuums, robot lawnmowers, sprinklers, lights, scenes, scripts,
  and other Home Assistant devices.
- Optionally delays a scheduled start while a matching work meeting is active.
- Adds a configurable safety buffer after the meeting ends.
- Handles overlapping or consecutive meetings.
- Uses an `input_boolean` helper as a voice-controllable pause/kill switch.

## Calendar examples

Create one automation from the blueprint for each distinct command:

- `[Rosie] Vacuum`
- `[Mower] Mow Front Yard`
- `[Sprinklers] Water`
- `[Pool] Clean`
- `[Lights] Patio On`

Each automation can run completely different Home Assistant actions. For a
multi-step activity, create a Home Assistant script and select that script as
the start action.

## Installation

Use the **Import Blueprint** button above, or paste this URL into Home
Assistant's blueprint importer:

```text
https://github.com/mn22680/HA-Calendar-Event-Start/blob/main/calendar_text_command.yaml
```

Manual installation path:

```text
/config/blueprints/automation/mn22680/calendar_text_command.yaml
```

After importing, open **Settings → Automations & scenes → Blueprints**, locate
**Calendar text command (pause + work-meeting delay)**, and select **Create
automation**.

## Example: robot lawnmower

- **Schedule calendar:** Weekly Family Schedule
- **Text:** `[Mower] Mow Front Yard`
- **Start action:** the mower's start/mow action or a control script
- **End action:** `lawn_mower.pause` or `lawn_mower.dock`
- **Pause helper:** an `input_boolean` such as `Mower activity paused`
- **Delay if Work Meeting:** enabled, if desired
- **Work calendar:** NoonanFilms.com

## Google voice control

Create a Home Assistant Toggle helper for each command or device and select it
as the blueprint's pause helper. Expose that helper to Google Assistant, then
create a Google Home Household Routine such as **“pause the mower”** that turns
on the helper.

The bare phrase **“Google, stop”** is generally consumed by Google/Nest as a
local media or alarm command. A specific routine phrase such as **“stop Rosie”**
or **“pause the mower”** is more reliable.

## Notes

- Home Assistant calendar integrations poll periodically. Create or edit test
  events at least 15 minutes before their start time.
- Turning on the pause helper interrupts the active blueprint run immediately.
- Turning it off resumes only when resume is enabled and a matching scheduled
  event is still active.
- This blueprint requires Home Assistant 2024.10 or newer.
