# hue-homeassistant
Hue scripts and things for home assistant.

## Blueprints

### TV Simulator

[![Open your Home Assistant instance and show the blueprint import dialog with this blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/create-link/?redirect=blueprint_import&blueprint_url=https%3A%2F%2Fgithub.com%2FJeroen-Priester%2Fhue-homeassistant%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Ftv_simulator.yaml)

Makes lights flicker as if a TV is on, for presence simulation. It is modelled on
[WLED's TV Simulator effect](https://github.com/wled/WLED/blob/main/wled00/FX.cpp):
- minutes-long "scenes" with a base colour
- short "shots" that vary around that colour
- random fades, with hard cuts about 30% of the time

It uses plain `light.turn_on` calls, so it works with any colour light. It needs no
Hue Entertainment streaming.

**Setup**
1. Import the blueprint using the button above, or from
   [`blueprints/automation/tv_simulator.yaml`](blueprints/automation/tv_simulator.yaml).
2. Create a toggle helper (Settings → Devices & services → Helpers → Toggle), e.g.
   `input_boolean.tv_simulator`.
3. Create an automation from the blueprint. Pick the toggle and the lights (single
   lights, areas or Hue rooms).

**Usage**
- Turn the toggle on to start and off to stop. By default, the lights turn off when it
  stops.
- If Home Assistant restarts, or the automation is edited, while the toggle is on, the
  simulation resumes within 2 minutes.
- Timing and look settings are in the collapsed sections of the automation.

**Hue rate limits**
- Individual lights: each light costs one bridge command per shot, and the bridge
  handles about 10 commands/s. Keep (number of lights) ÷ (shortest shot in seconds)
  under about 8.
- Hue rooms/zones: they take one command per shot but only about 1 command/s. Set the
  shortest shot to at least 1000 ms.

Requires Home Assistant 2024.10 or newer.
