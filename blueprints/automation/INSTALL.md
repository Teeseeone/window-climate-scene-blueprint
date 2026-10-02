# Window Open - Climate Off (Automatic Scene, No Helpers)

This adaptation creates and deletes its own temporary scene. You do not
need to create a text helper, boolean helper, or scene yourself.

## Install

1. Open **Settings > Automations & scenes > Blueprints > Import blueprint**.
2. Paste this GitHub URL, preview, and import it:
   https://github.com/Teeseeone/window-climate-scene-blueprint/blob/main/blueprints/automation/window-open-climate-scene.yaml
   Select **Window Open - Climate Off (Automatic Scene, No Helpers)** and choose
   **Create automation**.
3. Select your window sensor and climate device. Set the open and closed delays
   using each slider or its editable numeric field. Both range from 0 to 60 seconds
   in 1-second steps. Zero acts immediately. The defaults are 4 seconds, matching
   the screenshot. Once installed, configure these settings in the blueprint form;
   no YAML editing or external helpers are required.
4. Leave **Restore strategy** at **Restore the saved scene**. Leave **What to save**
   at **HVAC mode only** to preserve temperature changes made while paused; choose
   **All supported climate settings** to restore temperature, preset, and other
   settings that the climate integration supports through scenes.
5. Save and enable the new automation. Disable the old window/heater automation
   before testing so only one automation controls the same heater.

All day-to-day settings are configured through the blueprint form. No YAML
editing is needed. For a manual file install instead, copy the file into
`blueprints/automation/local/window-open-climate-scene.yaml` and reload blueprints.

## Behavior

- A window that stays open for the selected delay causes a running heater's state
  to be saved into a scene, then its HVAC mode is set to off.
- A heater that was already off gets no scene and remains off when the window closes.
- Closing restores the saved scene after the closed delay and deletes the scene.
- Reopening during the closed delay cancels restoration and retains the first scene.
- Different automation entities get different scene names, starting `scene.window_climate_`.
- An unavailable window never counts as closed. A recovering window sensor retries
  after the appropriate delay. If the climate device is unavailable, the current
  action waits for it to report a supported mode; another window state change
  cancels that wait.
- If the heater is already active when restoration is due, the saved scene is
  discarded and the heater's current settings are kept.
- Additional open/close actions run after pausing/restoring. They may be cancelled
  by another window state change. Open actions may repeat after reopening or reload.

## Scene lifetime and limits

Home Assistant removes dynamically created scenes after a restart or scene reload.
If this happens while the heater is off, the previous settings cannot be recovered;
the blueprint leaves the heater off until you turn it on again. This is not a
restart-safe design. A normal automation reload can resume a pause if the dynamic
scene still exists. Delays begin again after reload or availability recovery.

Keep the automation entity ID and selected heater unchanged while a scene is pending.
Do not run multiple window-control automations for the same heater; use one existing
binary-sensor group if several windows control a single heater.

A manual off command while the heater is already off cannot be distinguished from
the automation's off state. To cancel a pause deliberately, disable the automation
and delete its pending scene through the `scene.delete` action. Otherwise the next
qualifying window close restores that scene.

The blueprint checks that the climate device supports off. The optional auto strategy
also requires auto support. Full-setting scene restoration depends on the device's
integration. Home Assistant 2024.10 or newer is required.

## Verify on your heater

1. Start in heat mode; open the window longer than the open delay. Confirm a scene
   appears and the heater switches off. Close the window and confirm it restores.
2. Start with the heater off; open and close the window. Confirm it remains off.
3. While paused, close the window briefly and reopen it before the closed delay
   expires. Confirm it stays off and the original scene is retained.
4. With mode-only selected, change the target temperature while paused. Confirm
   closing restores the mode without resetting that new target temperature.

Local checks validate YAML, templates, and simulated automation behavior. This
file has not been imported into or tested against your live Home Assistant instance.

## Source and documentation

Adapted from the public selectable restore-mode blueprint by blaugrau90:
https://github.com/blaugrau90/home-assistant/blob/main/blueprints/automation/window-open-climate-restore-mode.yaml

Original concept by SmartLiving.Rocks:
https://community.home-assistant.io/t/window-open-climate-off/257293

The exact restart-safe/minimal-helpers blueprint in the screenshot was not available;
this adaptation uses the related public source discussed in this chat.

Scene lifetime: https://www.home-assistant.io/actions/scene.create/
