[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/S6S116FB5)

# Generic Hygrostat for Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)

Due to the fact that humidity levels are different during the summer and winter, a static humidity level switching the fan is on/off is not possible. This binary_sensor detects high rises in humidity and switches on. And switches off when the humidity is back to normal.

Inspired by:
https://www.domoticz.com/wiki/Humidity_control

## Setup
In your `configuration.yaml` you'll need:

```yaml
binary_sensor:
- platform: generic_hygrostat
  name: Bathroom Hygrostat
  sensor: sensor.bathroom_climate_humidityy # Source humidity sensor
  delta_trigger: 3 # Optional humidity swing to detect. Default = 3
  target_offset: 3 # Optional dehumidification target offset. Default = 3
  min_on_time: 300 # Optional min on time in seconds. Default = 0 seconds
  max_on_time: 7200 # Optional safety max on time in seconds. Default = 7200 seconds
  sample_interval: 300 # Optional time between taking humidity samples in seconds, default 300 seconds
  min_humidity: 30 # Optional minimum humidity to enable dehumidification. Default = 0
  unique_id: # An ID that uniquely identifies this sensor. Set this to a unique value to allow customization through the UI.
```
It will create a binary sensor called `binary_sensor.bathroom_hygrostat`.
Next, add some automations to switch your fan:

```yaml
automation:
- alias: Bathroom Hygrostat On
  trigger:
    platform: state
    entity_id: binary_sensor.bathroom_hygrostat
    to: 'on'
  action:
    - service: switch.turn_on
      entity_id: switch.fan

- alias: Bathroom Hygrostat Off
  trigger:
    platform: state
    entity_id: binary_sensor.bathroom_hygrostat
    to: 'off'
  action:
    - service: switch.turn_off
      entity_id: switch.fan
```


## Installation
### HACS [![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)
1. In HACS Store, search for [***basschipper/homeassistant-generic-hygrostat***]
1. Install the custom integration
1. Setup the generic hygrostat custom integration as described above

### Manual
1. Clone this repo
1. Copy the `custom_components/generic_hygrostat` folder into your HA's `custom_components` folder
1. Setup the generic hygrostat custom integration as described above
