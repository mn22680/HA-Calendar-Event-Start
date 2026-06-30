# HA Calendar Event Start

A reusable Home Assistant automation blueprint that runs actions from calendar
events whose titles contain selected text.

[![Open your Home Assistant instance and import this blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmn22680%2FHA-Calendar-Event-Start%2Fblob%2Fmain%2Fcalendar_text_command.yaml)

## What it does

- Watches any Home Assistant calendar, including Google Calendar.
- Separately matches either the text inside `[brackets]` or the text after `]`.
- Matches the selected title portion case-insensitively, exactly or partially.
- Provides configurable start, end, pause, and resume actions.
- Works with vacuums, robot lawnmowers, sprinklers, lights, scenes, scripts,
  and other Home Assistant devices.
- Optionally delays a scheduled start while a matching work meeting is active.
- Adds a configurable safety buffer after the meeting ends.
- Handles overlapping or consecutive meetings.
- Uses an `input_boolean` helper as a voice-controllable pause/kill switch.
- Optionally checks whether the device is disabled, has a low battery, or reports
  a full bin before starting.
- Can run charging/emptying actions and announce why a scheduled activity was blocked.
- Can announce a customizable warning before the scheduled action begins.

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

### Choosing which title part to match

For a calendar title such as `[Rosie] Vacuum`:

- Leave **Search after the closing bracket** off and enter `Rosie` to match the
  text inside `[ ]`.
- Turn **Search after the closing bracket** on and enter `Vacuum` to match only
  the action text after `]`.

Enter only one portion—never `[Rosie] Vacuum`. If exact matching is disabled,
`Vacuum` also matches action text such as `Vacuum First Floor`.

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

## Updates

The blueprint includes its GitHub `source_url` and displays its current version
at both the top and bottom of its configuration screen. Select **Check for an
updated version** there to open this repository.

Home Assistant does not currently auto-update imported blueprints. To install a
newer release:

1. Open **Settings → Automations & scenes → Blueprints**.
2. Find **Calendar text command** (the current version is shown in its name).
3. Open its three-dot menu.
4. Select **Re-import blueprint** and confirm the replacement.
5. Open automations created from the blueprint and confirm any newly added
   options. Existing selections are normally retained, while new options use
   their defaults.

> **Version 1.2.0 migration:** Existing automations may retain an older value
> such as `[Rosie] Vacuum`. Edit each automation after re-importing and change it
> to one part only, such as `Rosie` or `Vacuum`, then set the new bracket-position
> toggle accordingly.

You can also paste the original import URL into **Import Blueprint** again:

```text
https://github.com/mn22680/HA-Calendar-Event-Start/blob/main/calendar_text_command.yaml
```

On GitHub, select **Watch → Custom → Releases** to receive release notifications.
New features, suggestions, and problems can be posted in the repository's
[Issues](https://github.com/mn22680/HA-Calendar-Event-Start/issues) section.

## Status checks and announcements

Enable **Status Check** to configure any combination of:

- Device enabled/disabled state
- Numeric battery level and low-battery threshold
- Full/bin status and the state value that means full
- Optional low-battery or full handling actions

Enable **Announcements**, select a `tts` provider and one or more speakers, then
customize the pre-start and failed-check messages. Status failures prevent the
scheduled start action. The blueprint uses Home Assistant's modern `tts.speak`
action, making it suitable for Google/Nest speakers and other supported media
players.

## Optional feature sections

Pause/Kill, Work-meeting delay, Device status checks, and Announcements are
optional, collapsed sections. Expand only the feature you want and turn on its
**Enable** switch. Disabled sections are ignored by the automation and use safe
placeholder defaults where Home Assistant requires a syntactically valid entity.

Home Assistant's blueprint editor does not currently support dynamically hiding
individual fields when a Boolean input changes. Collapsed sections are therefore
used to keep disabled feature settings out of the way. The fields appear when a
section is expanded, but they have no effect until that section's enable switch
is turned on.

## Example: robot lawnmower

- **Schedule calendar:** Weekly Family Schedule
- **Text:** `Mower` with the bracket-position toggle off, or `Mow Front Yard`
  with the toggle on
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
