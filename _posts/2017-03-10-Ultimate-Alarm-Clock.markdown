---
layout:     post
title:      Ultimate Alarm Clock
author:     Ron Mar
tags: 		Home-Assistant lighting automation
subtitle:  	How-to Guide
category:  ha
---
<!-- Start Writing Below in Markdown -->

See how home automation can get you up in the morning. Watch the YouTube video here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/qJ8ZoJuuZVA" frameborder="0" allowfullscreen></iframe>

<h1 id="TOC">Table of Contents</h1>

* TOC
{:toc}

# Intro

Before you start, you'll need two things:
1. Raspberry Pi (and SD card and power supply) with Home Assistant installed
2. Devices to control

You can learn how to install Home Assistant from the <a href="https://home-assistant.io/docs/hassbian/installation/" target="_blank">Home Assistant Site</a> or this helpful <a href="https://www.youtube.com/watch?v=tCGlQSsQ-Mc" target="_blank">video by BRUH Automation</a>.

Different devices connect to Home Assistant in different ways. Some devices such as Philips Hue bulbs and Sonos Speakers can be automatically detected and configured by the <a href="https://home-assistant.io/components/discovery/" target="_blank">discovery component</a>. Z-Wave devices can be connected using an USB stick plugged into the Raspberry Pi and some <a href="https://home-assistant.io/docs/z-wave/" target="_blank">configuration in Home Assistant</a> (BRUH Automation has a <a href="https://www.youtube.com/watch?v=ajklDCaOGwY" target="_blank">great video</a> about this as well). 

# Interface

## Input Sliders

```
input_slider:
  alarm_clock_hour:
    initial: 6
    min: 0
    max: 23
    step: 1
  alarm_clock_minute:
    initial: 15
    min: 0
    max: 55
    step: 5
```

## Input Boolean

```
input_boolean:
  alarm_clock_status:
    initial: on
```

## Sensor

<div class="highlighter-rouge"><pre class="highlight"><code>  - platform: time_date
    display_options:
      - 'time'
  - platform: template
    sensors:
      alarm_clock_hour:
        value_template: '{<code></code>{ states.input_slider.alarm_clock_hour.state | int }}'
      alarm_clock_minute:
        value_template: '{<code></code>{ states.input_slider.alarm_clock_minute.state | int }}'
      alarm_clock_time:
        value_template: >-
          {<code></code>{ states.sensor.alarm_clock_hour.state }}:
          {<code></code>%- if states.sensor.alarm_clock_minute.state|length == 1 -%}
            0
          {<code></code>%- endif -%}
            {<code></code>{ states.sensor.alarm_clock_minute.state }}
      alarm_clock_time_long:
        value_template: >-
          {<code></code>% if states.sensor.alarm_clock_hour.state|length == 1 -%}
            0
          {<code></code>%- endif -%}
            {<code></code>{ states.sensor.alarm_clock_hour.state }}:
          {<code></code>%- if states.sensor.alarm_clock_minute.state|length == 1 -%}
            0
          {<code></code>%- endif -%}
            {<code></code>{ states.sensor.alarm_clock_minute.state }}</code></pre>
</div>

## Group

```
group:
# Alarm clock
  alarm_clock:
    name: 'Alarm Clock'
    entities:
      - sensor.alarm_clock_time
      - input_slider.alarm_clock_hour
      - input_slider.alarm_clock_minute
      - input_boolean.alarm_clock_status
```

## Customize

```
  customize:
    # Alarm clock sensors
    sensor.time:
      hidden: true
    sensor.alarm_clock_hour:
      hidden: true
    sensor.alarm_clock_minute:
      hidden: true
    sensor.alarm_clock_time_long:
      hidden: true
    sensor.alarm_clock_time:
      friendly_name: 'Alarm Clock Setting'
      icon: mdi:alarm
    # Alarm clock inputs
    input_slider.alarm_clock_hour:
      friendly_name: 'Hour'
      icon: mdi:timer
    input_slider.alarm_clock_minute:
      friendly_name: 'Minute'
      icon: mdi:timer
    input_boolean.alarm_clock_status:
      friendly_name: 'Alarm Clock Status'
      icon: mdi:alarm-check
```

# Script

```
script:
  wake_up:
    sequence:
      - service: light.turn_on
        data: 
          entity_id: light.lux_lamp
          brightness: 255
          transition: 10
      - service: homeassistant.turn_off
        entity_id: switch.smart_switch1
```

# Automation

<div class="highlighter-rouge"><pre class="highlight"><code>automation:
  - alias: 'Hue light on gradually with alarm'
    hide_entity: False
    trigger:
      platform: template
      value_template: '{<code></code>{ states.sensor.time.state == states.sensor.alarm_clock_time_long.state }}'
    condition:
      condition: state
      entity_id: input_boolean.alarm_clock_status
      state: 'on'
    action:
      service: script.wake_up</code></pre></div>

# Next Steps

## References
