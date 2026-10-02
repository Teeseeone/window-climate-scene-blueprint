# Window Open - Climate Off: Scene Version

This fork adds a blueprint that creates and restores its own temporary scene. No text helper, boolean helper, or manually created scene is required.

- Separate open and closed delays: **0-60 seconds**.
- Each delay has a **slider and editable numeric field**, with 1-second steps and a 4-second default.
- Restore only the HVAC mode, or all supported climate settings.
- An already-off heater stays off; reopening retains the original snapshot.

## Import through Home Assistant

Open **Settings > Automations & scenes > Blueprints > Import blueprint** and paste this [blueprint URL](https://github.com/Teeseeone/home-assistant/blob/main/blueprints/automation/window-open-climate-scene.yaml). Preview and import it, then create an automation and select your window sensor and heater. All settings are adjusted through the blueprint form. No YAML editing is needed.

[Detailed setup and behavior](blueprints/automation/INSTALL.md)

**Scene lifetime:** Home Assistant removes temporary scenes on restart or scene reload. If the scene is lost while paused, this blueprint leaves the heater off rather than guessing its previous settings.

Local YAML, template, and 17 behavior simulations passed. Live Home Assistant and heater testing remains pending.

Adapted from blaugrau90's selectable restore-mode blueprint, based on the original concept by SmartLiving.Rocks. Original blueprints are retained below.

---

# Home Assistant

Personal collection of Home Assistant blueprints, automations, and integrations.

---

## Blueprints

### Climate

| Blueprint | Description |
|---|---|
| [Window Open – Climate Off (Restore Mode)](blueprints/automation/window-open-climate-restore-mode.yaml) | Turns off a climate device when a window opens. When the window closes, choose to **restore the previous state** or **switch to HVAC mode `auto`**. Supports optional delay times and custom actions. |
| [Window Open – Climate Off (Advanced)](blueprints/automation/window-open-climate-advanced.yaml) | Extended version: choose to restore the previous state or **set a specific HVAC mode**. When not using `auto`, also control the temperature — restore the saved value, set a fixed target, or leave it unchanged. |
| [Heating Control – Away & Coming Home](blueprints/automation/climate-presence-control.yaml) | Controls all heating devices based on `zone.home`. Switches all heaters to the **Away preset** when nobody is home, and to the **Home preset** when someone returns. Supports optional window sensors per heater — open windows are skipped. |
| [Heating – Away & Home](blueprints/automation/climate-away-home.yaml) | Minimal blueprint: switches a single climate entity between **Away** and **Home** preset based on `zone.home`. Ideal for systems like Tado where one device controls the whole home. |

---

## Installation

Copy the desired YAML file into your Home Assistant config directory under the matching path, e.g.:

```
config/
└── blueprints/
    └── automation/
        └── window-open-climate-restore-mode.yaml
```

Then reload blueprints in Home Assistant: **Settings → Automations → Blueprints → Reload**.
