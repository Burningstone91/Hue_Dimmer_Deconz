# DeCONZ Hue Dimmer Switch

[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)

_Smooth dimming functionality (and more) for Hue Dimmer Switches and lights connected through DeCONZ_

## Installation

Download the `hue_dimmer_switch_deconz` directory from inside the `apps` directory here to your local `apps` directory, then add the configuration to enable the `hacs` module.

## App configuration

### Basic configuration:
With the basic configuration the behaviour is as follows:

* release ON button after short press of the button-> light turns on at 100%
* release OFF button after short press of the button-> light turns off
* press and hold INCREASE BRIGHTNESS button -> brightness increases smoothly
* press and hold DECREASE BRIGHTNESS button -> brightness decreases smoothly
* release INCREASE BRIGHTNESS or DECREASE BRIGHTNESS after holding the button -> dimming stops

##### Example config:

```yaml
dimmer_bedroom:
  module: hue_dimmer_switch_deconz
  class: HueDimmerSwitch
  entities:
    switch: dimmer_switch_bedroom
    light: light.bedroom
```
#### Configuration
key | optional | type | default | description
-- | -- | -- | -- | --
`module` | False | string | hue_dimmer_switch | The module name of the app.
`class` | False | string | HueDimmerSwitch | The name of the Class.
`entities` | True | map | | Light and switch entities.

##### Entities
key | optional | type | default | description
-- | -- | -- | -- | --
`switch` | False | string | | The id of the Hue Dimmer Switch.
`light` | False | string | | The entitiy_id of the light.

### Advanced configuration:
The switch behaves the same way as with the basic configuration. However, the button presses can be overwritten and other button presses can be added. The available actions to assign to a button press are “turn_on”, “turn_off” and “service_call”.

The configuration for a button press looks like this.
```yaml
button_press_name:
  action_type: turn_on | turn_off | service_call
  action_entity: entity_id of entity to be used in the action
  parameters: optional, parameters to give if you chose service_call
```
The following button_press_names are available and can be overwritten:

* short_press_on
* short_press_on_release
* long_press_on
* long_press_on_release
* short_press_up
* short_press_up_release
* short_press_down
* short_press_down_release
* short_press_off
* short_press_off_release
* long_press_off
* long_press_off_release

##### Example config:

```yaml
dimmer_test:
  module: hue_dimmer_switch
  class: HueDimmerSwitch
  entities:
    switch: dimmer_switch_bedroom (required)
    light: light.bedroom (required)
  advanced:
    short_press_on_release: (optional)
      action_type: turn_on
      entity: scene.good_night
    long_press_on_release: (optional)
      action_type: service_call
      entity: light.bedroom
      service: turn_on
      parameters:
        brightness: 255
        color_name: blue
    short_press_off_release (optional):
      action_type: turn_off
      entity: light.bedroom
    long_press_off_release (optional):
      action_type: turn_off
      entity: group.bedroom
```

#### Configuration
key | optional | type | default | description
-- | -- | -- | -- | --
`module` | False | string | hue_dimmer_switch | The module name of the app.
`class` | False | string | HueDimmerSwitch | The name of the Class.
`entities` | True | map | | Light and switch entities.
`advanced` | True | map | | Advanced button configuration, each entry needs to be one of the available button presses.

##### Entities
key | optional | type | default | description
-- | -- | -- | -- | --
`switch` | False | string | | The id of the Hue Dimmer Switch.
`light` | False | string | | The entitiy_id of the light.

##### Advanced
key | optional | type | default | description
-- | -- | -- | -- | --
`action_type` | False | string | | Type of actions to execute. Possible values _turn_on_, _turn_off_, _service_call_
`entity` | False | string | | The entitiy_id to be used for the action.
`service` | True | string | | The service to be used for the action if the chosen action type is _service_call_.
`parameters` | True | map | | Parameters if the chosen action type is _service_call_.
