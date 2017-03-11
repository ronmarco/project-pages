---
layout:     post
title:      Ultimate Alarm Clock
author:     Ron Mar
tags: 		Home-Assistant lighting automation
subtitle:  	How-To Guide
category:  ha
---
<!-- Start Writing Below in Markdown -->

See how home automation can get you up in the morning. Watch the YouTube video here:

<div align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/qJ8ZoJuuZVA" frameborder="0" allowfullscreen></iframe></div>

<h1 id="TOC">Table of Contents</h1>

* TOC
{:toc}

# Intro

Before you start, you'll need two things:
1. Raspberry Pi (and SD card and power supply) with Home Assistant installed
2. Devices to control

You can learn how to install Home Assistant from the [Home Assistant Site](https://home-assistant.io/docs/hassbian/installation/) or this helpful [video by BRUH Automation](https://www.youtube.com/watch?v=tCGlQSsQ-Mc).

Different devices connect to Home Assistant in different ways. Some devices, such as Philips Hue bulbs and Sonos Speakers, can be automatically detected and configured by the [discovery component](https://home-assistant.io/components/discovery/). Z-Wave devices can be connected using a USB stick plugged into the Raspberry Pi and some [configuration in Home Assistant](https://home-assistant.io/docs/z-wave/) (BRUH Automation has a [great video](https://www.youtube.com/watch?v=ajklDCaOGwY) about this). 

My alarm clock was originally inspired by this [thread on the Home Assistant forums](https://community.home-assistant.io/t/creating-a-alarm-clock/410).

# Interface

The first step setting up the interface in home assistant. It is used to set the time for the alarm and once you set it to the right time, you won't have to touch it again.

## Input Sliders

We use input sliders to set the hour and minute of the alarm clock. See below for code to paste in your **configuration.yaml** file:

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

An input boolean is an easy way to make an on/off switch for the alarm:

```
input_boolean:
  alarm_clock_status:
    initial: on
```

## Sensor

Now we need to add new "sensors" that format the input slider values and the current time to make them easier to compare. There are several steps:

1. Locate the `sensor:` line in your **configuration.yaml** file. The code will go below this line.
2. If there is an existing sensor such as `platform: yr`, add a dash and a space before it to begin a list (i.e., it should change to `- platform: yr`).
3. Paste the code for the new sensors on the next line. The code is shown below:

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

Now that we have the inputs created, we'll make the interface a little more organized. This code groups the alarm clock inputs into a single card in Home Assistant:

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

This next code hides "sensors" that aren't helpful to see and gives icons and better-looking names to the ones we do want to see. We have to scroll up to the top of the **configuration.yaml** file to paste code under `homeassistant:`.

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

Now we'll write the script that will tell Home Assistant what to do when the alarm goes off:

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

Personalize the above code based on your own configuration:

* Substitute the entity names of your own devices for `light.lux_lamp` and `switch.smart_switch1`.
* Change the `service:` to fit your needs. Lights can be turned off with `light.turn_off` and entities can be turn on with `homeassistant.turn_off`
* When editing the `light` service, the `brightness:` parameter sets the brightness of the bulb on a 0-255 scale and `transition:` sets the number of seconds to fade in.

# Automation

This last step ties the interface and the scripts together. The automation detects a trigger, such as being a certain time of day, and then runs a action, in this case `script.wake_up`. The `condition:` prevents the alarm clock from going off when it was turned off in the interface with `input_boolean.alarm_clock_status`.

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

There are tons of ways to take this alarm clock further, including:

For the interface, you could use Tasker or one of the many [Home Assistant components](https://home-assistant.io/components/#all) to set the alarm clock automatically using information from your phone. 

For the scripts, you can adjust timing. For example, adding the line `- delay: 00:05:00` before a service will delay it by 5 minutes ([more information here](https://home-assistant.io/docs/scripts/)). 

For the automation, you can create conditions to prevent the alarm from going off on weekends.

[This thread on the Home Assistant forums](https://community.home-assistant.io/t/creating-a-alarm-clock/410) inspired my alarm clock and has many more great ideas.
