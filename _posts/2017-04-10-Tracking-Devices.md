---
layout:     post
title:      Tracking Phones and Other Devices
author:     Ron Mar
tags: 		Home-Assistant Configuration Setup
subtitle:  	How-To Guide
category:  ha
---
<!-- Start Writing Below in Markdown -->

See how I track devices on my network and use the status in automations. Watch the YouTube video here:

<div align="center">
<iframe width="560" height="315"
src="https://www.youtube.com/embed/udt8MX86dcQ" frameborder="0" allowfullscreen>
</iframe>
</div>

There are tons of ways to track devices using Home Assistant. I'll walk you through two and introduce you to a few more.

<h1 id="TOC">Table of Contents</h1>

* TOC
{:toc}

# Track Devices with Your Wireless Router

The simplest way to track devices is using your wireless router 

I have a [tp-link router](http://amzn.to/2oqzG65). To use it within Home Assistant, I inserted the following code in my `configuration.yaml` file.

```
device_tracker:
  - platform: tplink
    host: YOUR_ROUTER_IP
    username: YOUR_ADMIN_USERNAME
    password: YOUR_ADMIN_PASSWORD
```

For more details, check out the [this page on the Home Assistant site](https://home-assistant.io/components/device_tracker.tplink/).

Here are components for other routers:

* [DD-WRT](https://home-assistant.io/components/device_tracker.ddwrt/)
* [Cisco IOS](https://home-assistant.io/components/device_tracker.cisco_ios/)
* [Linksys Access Points](https://home-assistant.io/components/device_tracker.linksys_ap/)
* [Netgear](https://home-assistant.io/components/device_tracker.netgear/)
* [Check this page](https://home-assistant.io/components/device_tracker/) for additional routers.

# Customizations

## Customize Device Tracker

Once the tracker is set up, it can be further configured with parameters to control how it discovers new devices and considers devices as "home".

Note that these work with any `device_tracker` platform, but if you have multiple platforms, Home Assistant will use the parameters from the first as global settings. [Find available parameters here](https://home-assistant.io/components/device_tracker/#configuring-a-device_tracker-platform). 

## Customize Known Devices

Once you discover devices, you can customize their appearance by editing the object name, friendly name, picture, and other details.

[Find a list of customizations here](https://home-assistant.io/components/device_tracker/#known_devicesyaml). 

# Nmap

Nmap (a.k.a., [Network Mapper](https://en.wikipedia.org/wiki/Nmap)) discovers hosts by reaching out to devices on your network.

Two steps are required to install:

1. Install Nmap on your host device (I use a [Raspberry Pi](http://amzn.to/2plPtjX)). I use the Hassbian image, which is based on Debian, so I use the following code on my Raspberry Pi: `sudo apt-get install net-tools nmap`
2. Add the Nmap tracker component to Home Assistant. I added the following code to my `configuration.yaml` file:

```
device_tracker:
  - platform: nmap_tracker
    hosts: IP_ADDRESSES_TO_SCAN
```

I also added the parameters `home_interval: 10` to limit how often nmap scans my devices and `exclude: HOME_ASSISTANT_HOST_IP` to prevent my Raspberry Pi from trying to scan itself, which can cause problems.

Find more details about the `nmap_tracker` platform [here](https://home-assistant.io/components/device_tracker.nmap_tracker/).

# Other Device Tracking Methods

You can also track devices using:

[Bluetooth](https://home-assistant.io/components/device_tracker.bluetooth_tracker/): great if Home Assistant is already running on a Raspberry Pi with Bluetooth.

[OwnTracks](https://home-assistant.io/components/device_tracker.owntracks/): Can provide specific locations outside your home using a phone GPS. Uses the [MQTT protocol](https://home-assistant.io/components/mqtt/).

[iCloud](https://home-assistant.io/components/device_tracker.icloud/): also works with Home Assistant.

[Additional methods are listed on the Home Assistant site](https://home-assistant.io/components/device_tracker/#configuring-a-device_tracker-platform).

# Tracking Un-Networked Devices

Using current sensors, I can evaluate the status of electronics not connected to my network, for example my computer monitor and TV.

I use the following code to translate a reading of >10 Watts from my Smart Sensor to an on/off status:

<div class="highlighter-rouge"><pre class="highlight"><code>binary_sensor:
  - platform: template 
    sensors:
      tv_status:
        friendly_name: 'TV Status'
        value_template: "{<code></code>{ states.switch.smart_switch1.attributes.current_power_w  > 10 }}</code></pre>
</div>

[Find more information on template binary sensors here](https://home-assistant.io/components/binary_sensor.template/).

# Creating Automations Using Tracked Devices

I primarily use device status to control my automations in two ways

1. Trigger for automations - start automations
2. Condition for automations - control when they running

Triggers are helpful when you want an automation to run when you arrive home, for example. You can also log the status with other Home Assistant components, such as [IFTTT](https://home-assistant.io/components/ifttt/).

Conditions can be used to prevent certain automations from running when your other half is home.

I hope this article was helpful. Let me know what else you're interested in!