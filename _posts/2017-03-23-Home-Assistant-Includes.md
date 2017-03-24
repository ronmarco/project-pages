---
layout:     post
title:      Split Up Home Assistant Configuration Using Includes
author:     Ron Mar
tags: 		Home-Assistant Yaml Configuration
subtitle:  	How-To Guide
category:  ha
---
<!-- Start Writing Below in Markdown -->

See how to make your Home Assistant configuration more organized. Watch the YouTube video here:

<div align="center">
<iframe width="560" height="315"
src="https://www.youtube.com/embed/Ili8B4hgQ-g" frameborder="0" allowfullscreen>
</iframe>
</div>

<h1 id="TOC">Table of Contents</h1>

* TOC
{:toc}

# Intro

If you are getting started with Home Assistant, you may notice the `configuration.yaml` file growing long and difficult to manage. I'm going to show you three ways I split my `configuration.yaml` file into other yaml files to make my configuration more organized. 

The Home Assistant website has a [great page on how to split your configuration](https://home-assistant.io/docs/configuration/splitting_configuration/). I'll be covering a more detailed how-to in this article.

The example configuration I use in this article (including the Alarm Clock) was created [here](http://ronmar.co/ha/2017/03/10/Ultimate-Alarm-Clock/).

# Includes in same folder

We'll separate some code from `configuration.yaml` into another yaml file in the same `/homeassistant/` folder. In this example, I'll move the code in the `group:` section, but it works for other sections in the `configuration.yaml`.

I start with this section of code:

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

**Step 1:** Create a new `groups.yaml` file with all the code after the header:

```
## Alarm clock

  alarm_clock:
    name: 'Alarm Clock'
    entities:
      - sensor.alarm_clock_time
      - input_slider.alarm_clock_hour
      - input_slider.alarm_clock_minute
      - input_boolean.alarm_clock_status
```

Note: I added a comment at the top so I can easily tell the purpose of the file.

**Step 2:** Add `!include groups.yaml` to the groups section header like this:

```
group: !include groups.yaml
```

That's it!

# Includes in sub-folder

If you split each section in the configuation file, there would be a lot of yaml filesin one folder! Fortuantely it's easy to organize with sub-folders in Home Assistant.

To place the yaml code in a sub-folder, all you have to do is:

**Step 1:** Move the `groups.yaml` file into a sub-folder within the `/homeassistant/` folder

**Step 2:** Add the folder name to the include code. The line in `configuration.yaml` should read:

```
group: !include include/groups.yaml
```

# Includes in merged folder

When your configuration gets very complex, it can be helpful to further split the yaml files. I'll use the `sensor:` section as an example.

I started with this code:

<div class="highlighter-rouge"><pre class="highlight"><code>Sensor:
  - platform: yr
  - platform: time_date
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

**Step 1:** Create a new folder in the `/homeassistant/` folder. For this exmaple, I'll use a folder named `sensor`.

**Step 2:** Add the code to include as yaml files in the new folder. Name the yaml files anything you want; Home Assistant only needs the folder name.

I'll create `weather.yaml` for the sensor related to a weather forecast:

```
## Weather sensor

  - platform: yr
```

The rest of the code is for an alarm clock. I'll add it to it's own `alarm-clock.yaml` file:

<div class="highlighter-rouge"><pre class="highlight"><code>## Alarm clock sensor

  - platform: time_date
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

**Step 3:** Add the following code to `configuration.yaml` file.

```
sensor: !include_dir_merge_list sensor/
```

Note: the sensor code is a 'list' becuase the code in each include begins with a dash (`-`). If you are including code that does not begin with a dash, use the word named (i.e.,`!include_dir_merge_named`). The `configuration:` section is not a "list" for example.

I hope this helps!
