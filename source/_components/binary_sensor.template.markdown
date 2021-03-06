---
layout: page
title: "Template Binary Sensor"
description: "Instructions how to integrate Template binary sensors into Home Assistant."
date: 2016-02-25 15:00
sidebar: true
comments: false
sharing: true
footer: true
ha_category: Binary Sensor
---

The `template` platform supports sensors which breaks out the state and `state_attributes` from other entities.

To enable Template binary sensors in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  platform: template
  sensors:
    sun_up:
      value_template: {% raw %}'{{ states.sun.sun.attributes.elevation > 0}}'{% endraw %}
      friendly_name: 'Sun is up'
```

Configuration variables:

- **sensors** array (*Required*): List of your sensors.
  - **friendly_name** (*Optional*): Name to use in the Frontend.
  - **sensor_class** (*Optional*): The [type/class](/components/binary_sensor/) of the sensor to set the icon in the frontend.
  - **value_template** (*Optional*): Defines a [template](/topics/templating/) to extract a value from the payload.
  - **warnings** (*Optional*): Turn off warnings (useful if the sensor is loaded before devices it depends on).
  - **entity_id** (*Optional*): Add a list of entity_ids so the sensor only reacts to state changes of these entities. This will reduce the number of times the sensor will try to update it's state.

## {% linkable_title Examples %}

In this section you find some real life examples of how to use this sensor.

### {% linkable_title Sensor threshold %}

This example indicates true if a sensor is above a given threshold. Assuming a sensor of `furnace` that provides a current reading for the fan motor, we can determine if the furnace is running by checking that it is over some threshold:

```yaml
sensor:
  platform: template
  sensors:
    furnace_on:
      value_template: {% raw %}{{ states.sensor.furnace.state > 2.5 }}{% endraw %}
      friendly_name: 'Furnace Running
      sensor_class: heat
```

### {% linkable_title Switch as sensor %}

Some movement sensors and door/window sensors will apear as a switch. By using a template binary sensor, the switch can be displayed as a binary sensors. The original switch can then be hidden by [customizing.](/getting-started/customizing-devices/)

```yaml
binary_sensor: 
  platform: template 
  sensors:
    movement:
      value_template: {% raw %}"{{ states.switch.movement.state == 'on' }}"{% endraw %}
      sensor_class: motion
    door:
      value_template: {% raw %}"{{ states.switch.door.state == 'on' }}"{% endraw %} 
      sensor_class: opening
```
