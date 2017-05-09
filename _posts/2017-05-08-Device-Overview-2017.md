---
layout:     post
title:      Getting Started with Home Automation in 2017
author:     Ron Mar
tags: 		Home-Assistant Configuration Setup
subtitle:  	How-To Guide
category:  ha
---
<!-- Start Writing Below in Markdown -->

You're relaxing on the couch and just as you are getting comfortable, you realize it's too cold and there's glare on the TV. You have to get up, adjust the thermostat, and turn off the lights.

Fortunately, this First World problem can be solved with home automation! Unfortunately, the big companies trying to force customers onto their proprietary home automation platforms has led to a confusing, fragmented market. I'll try to make sense of it all in this article.

I'll talk about the different high-level solutions for home automation, and help you decide what's best for your own needs. I'll order solutions from least time-consuming to the most demanding. 

<h1 id="TOC">Table of Contents</h1>

* TOC
{:toc}

# Overview

I am going to talk about everything in consumer home automation from professional solutions to coding your own setups. As a caveat, the info I will share are my opinions. I haven't personally used every product I mention, but this information is something I wish I had when I was beginning with home automation.

Home automation is a tradeoff between performance you get and the investment you have to make. I will rank the solutions with the following considerations (high is good):
* Performance
  * Features/Interface - What it can do
  * Compatibility - How well it works with other devices
* Investment
  * Affordability  - Relative cost of equipment
  * Time-Efficiency - Time needed to set up and troubleshoot

# Professional Solutions

The easiest way to get something done is to throw money at it - and Professional Solutions are the way to do that in home automation. You don't hear much about this on online because the products are marketed towards general contractors who then sell to homeowners. 

Typically you would talk to a contractor, mention that features you want, and they would install it for you. 

Companies offering these solutions also sell building management systems that are installed in commercial buildings. They include:
*Crestron
*Lutron
*Vantage Controls

*Note: The above companies have recently entered the DIY home automation market as well.*

Contractors will specialize or be certified in a specific brand, so choosing a brand doesn't matter as much. 

**Features/Interface: High**

You can automate almost anything in your house if you pay for it. Interfaces are typically well designed and simple to use. Because of the background with commercial building automation, the products are very reliable.

**Compatibility: Low**

There is some compatibility with other systems, but the protocols used are typically proprietary, so it won't simply work many off-the-shelf home automation components. Companies have been making improvements as more home automation devices enter the market, but they are still less compatible than other solutions. Setups can be built into house - and hardwired - so modifications are physically difficult. 

**Affordability: Very Low**

As you can imagine, the costs are very high. Expect to pay four, five, even six figures for a very nice setup. Remember, you're paying for labor too.

**Time-Efficiency: High**

You're outsourcing the installation, so you're not doing anything yourself. Additionally, dedicated support is often included, which can limit the time you spend troubleshooting.

**Summary**

As a grad student living in a rented apartment, this isn't the best option for me. But if you have more money than time, this could be the way to go.

# Single Purpose Devices

For most people, this is the best place to start. These products typically include a well-designed smartphone app that allows remote control and simple automations. Additionally, they're geared towards everyday consumers, so a lot of time went into making them easy to set up and use.

To start, pick a single thing you'd like to automate:

* Lights
* HVAC - heating or air conditioning
* A specific appliance - with a smart switch
* Home audio
* Others

User-friendly products are sold for all the above use cases:

Lighting - LIFX bulbs, Philips Hue, IKEA Tradfri

HVAC - Nest or Honeywell

Smart switch - Wemo

Home audio - Sonos devices

**Features/Interface: Medium**

Products are simple and intuitive, but the ability to automate is limited in most of the provided apps.

**Compatibility: High**

Although they often use proprietary protocols so you can't add devices to say a Philips Hue hub. Because these are popular products, almost all third party home automation products can control them. 

**Affordability: Medium**

These solutions are not the cheapest way to accomplish lighting tasks, but the usability of the app and installation is likely worth the premium when you're starting out.

**Time-Efficiency: High**

You're outsourcing the installation, so you're not doing anything yourself. Additionally, dedicated support is often included, which can limit the time you spend troubleshooting.

**Summary**

If you want to get a glimpse of what's possible with home automation without committing to a complex setup - this is a great way to get started.

# Cloud Services for Automation and Personal Assistants

These services help create automations for devices - and get a taste of what's possible in home automation - without being very costly or difficult to set up.

Examples:

* IFTTT - great introduction to the logic of home automation - allows users to create conditional automations using a simple drag-and-drop interface.

* Amazon Echo and Google Home - enable voice control through Amazon Alexa and Google Assistant - makes your setup more intuitive for your family to control.

**Features/Interface: Medium**

It's in the cloud - not on your network - so it can take time to trigger - and won't work if you have connectivity issues. They feature intuitive user interfaces.

**Compatibility: Medium**

A lot of compatible devices exist.

**Affordability: Medium/High**

IFTTT is free. Intelligent speakers like the Amazon Echo Dot can be cheap.

**Time-Efficiency: Medium**

You can spend hours adjusting parameters to get it to work as you want.

**Summary**

If you want to try advanced automations without having to code, this is a great way to get started.

# Home Automation Hubs

A hub brings all your home automation together - it is a central point that connects your devices. It also connects with your network and the internet.

Hubs talk to devices, using protocols - similar to how your wireless router talks to laptops and cell phones using wifi:

* Z-wave is the closest thing home automation has to a standard - there are a lot of products available 
* Zigbee is the second most common

When shopping for home automation devices, all you have to know is the protocol name - so you can check whether your hub is compatible.

You may wonder why home automation can't just use wifi. Well, you can, but the Z-wave and Zigbee are better in almost every way: they are cheaper, use less power, and they often have better connections because the devices pass messages along between each other (i.e., they form a "mesh network").

# Commercial Hubs

These are device hubs sold by a company. They will let you control a house worth of devices using home automation protocols. These are typically cloud-based and need an internet connection, which makes it less ideal for applications like security or unlocking your house.

Examples:

Here are a few examples of commercial hubs:
* Samsung Smartthings - known for being scalable
* Wink hub - known for easier setup
* Homseeer
* Harmony
* Insteon
* Vera control Veralite
* Many others

**Features/Interface: High**

Pretty easy to configure. Allow automation functions, but they can be difficult to work with and may require playing around. They offer dedicated customer service and well-developed communities you can go to.

**Compatibility: Medium/High**

The examples I mentioned have strong compatibility because they are key selling points of the products. However, they are proprietary platforms after all, so they don't work perfectly with all other products. If something new comes out, you have to wait for the company to update.

**Affordability: Medium**

They can be expensive, especially because commercial smarthubs seem to become obsolete after a couple of years.

**Time-Efficiency: Medium**

Commercial hubs require research and tinkering, but because they have strong development, you usually won't be spinning your wheels for long when troubleshooting.

**Summary**

Commercial hubs are a let you control all the devices in your home with a relatively compatible and user-friendly hub.

# Open Source Hubs

Open source hubs are typically developed by volunteers. They often rely on coding and tinkering with Unix or Linux operating systems. Because they aren't as influenced by commercial interests, they feature much wider compatibility and amazing capabilities.

Popular examples:

* Home Assistant
* OpenHAB

**Features/Interface: High**

Does not offer dedicated support - since it is open-source and a company isn't making money off it. But the communities are great - for Home Assistant and there are YouTubers like DIYAutomate, BRUH Automation, and me that try to teach how to get the most out of the hub

**Compatibility: Very High**

Every automation product I've owned, even uncommon, old IR blasters, have been supported by home automation. People made components for FedEx, USPS, and ride sharing apps Uber and lift - automations could be triggered by whether a package arrives or which ride sharing service is cheaper. They even have a component that checks whether the international space station component is above your house, I'm still thinking about how to incorporate that into my setup.

Chances are there's a coder with any home automation device you own, and it is either already compatible, or compatibility is about to be released.

**Affordability: Very High**

The software is offered for free.

**Time-Efficiency: Low**

You'll have to be familiar with coding or open to learning. The hubs have a bit of a learning curve and will take time for a new user to fully figure out.

**Summary**

Open source hubs will give you the best performance, but in exchange for your time.

# Recommendations

You can start at any of these points.

Personally, I took all the DIY steps. I used a Philips hue for a while. Then I moved on to a Vearlite Z-wave hub, which was the hot smart hub at the time. Currently I use the open source Home Assistant - Which still communicates with my Hue and my Veralite. But Home Assistant is the shot caller - it runs the interface and runs all of my automations.

Once I learned how to code in the language Home Assistant uses - YAML - I'm never going back to the commercial solutions. Coding is much quicker and less frustrating when creating automations.

If coding intimidates you, start out with the least time intensive tasks, you can see if the return is worth it and if you should still continue. The last thing I want is to have someone start out with home automation using a open source hub and stop because of the difficulty.

# Final Thoughts

I hope this helps make everything a little clearer!