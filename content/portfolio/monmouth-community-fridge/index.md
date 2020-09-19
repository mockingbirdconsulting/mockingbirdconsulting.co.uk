---
title: "Improving the safety of food redistribution through the Internet of Things"
type: portfolio
date: 2020-09-28T06:00:12+01:00
description: ""
caption: ""
featured: yes
projectDuration: Half a Day for installation, support and data analysis is on-going
casestudy_type: managed_iot
image: portfolio/monmouth-community-fridge/mcflogo.jpg
images: 
  - portfolio/monmouth-community-fridge/mcflogo.jpg
client: Monmouth Community Fridge
location: "Monmouth, Monmouthshire, UK"
category: ["IoT"]
startDate: September 2020
liveLink: "https://www.facebook.com/groups/monmouthcommunityfridge/" 
resources: 
  - name: mcflogo
    src: mcflogo.jpg
  - name: dashboard
    src: dashboard.png
  - name: fridges
    src: fridges.png
  - name: sensor
    src: sensor.png
tags:
  - Rural IoT
  - IoT Managed Infrastructure
  - Managed IoT
---
Monmouth Community Fridge is a volunteer group who redistribute nutritious, surplus food from local businesses to help prevent food waste in Monmouth. Based on the same site as Mockingbird Consulting's office and with such a great cause at the heart of it, we couldn't wait to help remove some of the more mundane tasks for the volunteers and improve food safety at the same time.

{{< bundle-image name="fridges" caption="The Community Fridge(s)">}}

The setup consists of three fridges and a freezer unit, and the existing setup had a number of digital thermometers that would be read and recorded by the volunteers twice a day.

We'd spoken before with the group about WiFi or Bluetooth thermometers, however as the fridges were housed in the old stable block with solid brick walls about 300mm thick it was clear that an alternative radio technology would be needed.  Thankfully, we're LoRaWAN experts and the same radio technology that can send [data about crops and weather](/portfolio/brookhousefarm/) over long distances is also perfect for sending data over short distances through Grade II listed walls!

As a starting point, we placed an ELSYS ESM5K sensor into the freezer and connected it to our platform.  Instantly, we started to collect data on the temperature, humidity, and light conditions inside the fridge using the free tier dashboards from [Datacake](https://datacake.co).

{{< bundle-image name="dashboard" caption="The metrics from the sensor before the freezer was switched on">}}

Once we had proved the sensor data was being recorded properly, we configured alerts within Datacake to send emails when the temperature rises above -18.5°C to ensure there is time for the community volunteers to act before breaching the Food Standards Agency guidelines on storing froze food, and a second alert when the light levels are above 0lux for more than 5 minutes.  Why do we monitor the light levels inside a freezer? So we know when the door has been left open!

We're delighted that we were able to donate our time and resources to the project, and we wish the team behind the [Monmouth Community Fridge](https://www.facebook.com/groups/monmouthcommunityfridge/) all the best for the future!
