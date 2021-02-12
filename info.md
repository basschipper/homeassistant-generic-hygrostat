[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/S6S116FB5)

# Generic Hygrostat for Home Assistant

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
  max_on_time: 7200 # Optional safety max on time in seconds. Default = 7200 seconds
  sample_interval: 300 # Optional time between taking humidity samples in seconds, default 300 seconds
  subsq_shower_detect: y # Optional. y = yes, anything else will be ignored. Resets value set by max_on_time if subsequent shower use is detected
  min_humidity: 30 # Optional minimum humidity to enable dehumidification. Default = 0
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
#### Manual
Put the [`binary_sensor.py`](generic_hygrostat/binary_sensor.py) in your home-assistant config directory under `custom_components/generic_hygrostat`.
#### Custom Updater
[`hygrostat_updater.json`](hygrostat_updater.json) provides the details Custom Updater needs. See [Custom Updater Installation](https://github.com/custom-components/custom_updater/wiki/Installation) to install it.

Add the following to your configuration:
```yaml
custom_updater:
  track:
    - components
  component_urls:
    - https://raw.githubusercontent.com/basschipper/homeassistant-generic-hygrostat/master/hygrostat_updater.json
```
