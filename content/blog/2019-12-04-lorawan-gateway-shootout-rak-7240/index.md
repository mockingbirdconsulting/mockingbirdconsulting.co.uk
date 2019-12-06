--- 
title: "The LoRaWAN Gateway Shoot-out - the Multitech Conduit"
date: 2019-11-06T00:00:00+00:00 
description: "How does the Multitech Conduit LoRaWAN Gateway stack up against our criteria?"
tags:
  - LoRaWAN 
  - IoT
  - Gateways
  - Multitech
resources:
  - name: header
    src: multitech.jpg
  - name: presets
    src: presets.png
  - name: status
    src: gatewaystatus.png
  - name: alerting
    src: DeviceHQAlerts.png

---
The technology that underpins our [farm and estates management solution](https://www.mockingbirdconsulting.co.uk/) is called LoRaWAN.

We're comparing various models of LoRaWAN Gateways to help you work out which is best for your project.

{{< bundle-image alt="The RAK7240 Gateway" name="header" caption="The RAK 7240 Gateway" >}}

This week, we're looking at the RAK 7240 - an outdoor gateway that retails for around £500 and available via RAK resellers.  This gateway is **not** available on the RAK website.

Power is *only* supplied via Power over Ethernet (PoE), so an ethernet cable will still need to be run and a PoE injector needs to be present in the system, however the PoE option means that unlike the [Laird RG1xx we reviewed previously](/blog/2019-10-07-lorawan-gateway-shootout-laird-rg186/) you can connect these gateways to the internet with just a single cable.

The RAK7240 also has GPS built in with a dedicated external antennae, making it possible to use triangulation between multiple gateways to remove the need for GPS on devices and use [Time Difference of Arrival](https://lora-alliance.org/sites/default/files/2018-04/geolocation_whitepaper.pdf) between gateways to locate a sensor.

Results on TDOA from others we know who are using this in practice appear to be mixed, however the GPS on the gateway allows us to be confident that the clock is set correctly and that it is showing in the right location on the gateway maps on the various platforms we use.

We'll be judging it against [this set of criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), so let's see how it compares...

### The environment in which it will live

The Conduit is designed to be an out gateway, and comes in a metal housing with an official rating of IP-67.

This casing makes it perfect for outdoor or other challenging environments such as dusty factory floors and rooms that are subject to high humidity.

### The different types of connectivity available

The unlike the [Multitech Conduit](/blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/), the RAK7240 comes with WiFi, LTE, and Ethernet (over the previously mentioned PoE port) by default, making it a great choice when you're not quite sure what the connectivity looks like at the remote site.


#### Ethernet (standard networking)

The ethernet port is situated on the underside of the unit within an IP-67 rated socket, providing easy routing of cables down from any mounting point.  As with the [Multitech Conduit](/blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/) the 10/100Mbps is slower than most current networking hardware, however unless you have hundreds or thousands of devices sending data via the same gateway, it is unlikely this will cause a bottleneck.

The ethernet port also provides 802.11af compatible Power over Ethernet to supply power to the device, meaning that you need a PoE injector.  The good news is that if you have a PoE switch already in place (and most enterprise organisations will have this these days), you can plug a single cable between the gateway and your switch and have that one cable carry both data *and* power.

The ethernet port forms a central part of the initial setup so you'll need a PoE injector just to get up and running with this one, however unlike the [Multitech Conduit](/blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/) DHCP is enabled by default, so the device will automatically obtain an IP Address. 

Check your router for the IP Address that has been assigned, visit it in your browser, and log in.  The default credentials are `root:root`.

#### LTE (4G Cellular)

As the model we are using will end up on our demo farm, we're checking that relying on LTE for our alternative connectivity back to the LoRaWAN servers will work for all our other customers.  This requires a SIM card from a mobile provider (not supplied), and therefore an additional monthly cost.  The gateway is also not configured to connect to any particular network as standard, so you'll need to find the APN and other data settings for your mobile provider in order to connect via LTE.

So far, we've found the LTE to be just as reliable as the ethernet, and the failover between the two connections when we disconnect the network supply from the PoE injector is flawless including the re-establishing of the VPN connection.

#### Conclusion

The options to either hard-wire or connect via LTE make this unit highly flexible, and the addition of LTE is a massive plus however. We'll be shipping all of our gateways with SIM cards installed and enabled so that in the event of a network outage the customer data continues to be collected.

### How easy is it to configure?

The RAK 7240 runs OpenWRT - an operating system we became familiar with when working on the [Bristol Wireless](https://www.bristolwireless.net/) project - and as a result, it has access to thousands of software packages.  Most importantly, it has the Luci dashboard and SSH built in and enabled by default, along with **native** support for [Chirpstack](https://chirpstack.io) - the Open Source platform that powers our LoRaWAN solution.

#### Initial setup

Setting up the RAK 7240 was a breeze.

We logged into the web interface, configured the LoRaGateway MQTT Bridge settings with our MQTT endpoint, username, and password, configured the LoRa Packer Forwarder to point to the local gateway bridge, saved the settings, and watched the data start to arrive into our Chirpstack console.

All in all, it probably took us about as long to configure LoRaWAN on the RAK as it did to reboot the Multitech just once.

We then went on to configure the LTE and WiFi interfaces just as fast, with just one reboot (taking about 1 minute) required in order to save the settings.

### How easy is it to manage once deployed?

Just like The Multitech Conduit, the RAK 7240 runs linux and comes with support for OpenVPN built in, however as we use both certificate and username/password authentication for our support VPN configuring the connection required the extra step of adding a file with the credentials to the device.  Unlike the Multitech, saving these details simply restarted the service instead of the entire device, ensuring that the data continued to flow whilst the changes were being made.

Once the VPN is configured, a test of failover between the LTE and Ethernet connections revealed a very quick switchover and reconnect, providing significant reassurance that once installed these devices will continue to "phone home" to our support server and seamlessly switch between the primary and backup connectivity options.

============================================= PICK UP FROM HERE =========================================

Monitoring of the RAK 7240 is relatively straight forward - we already have an MQTT connection from the gateway for our LoRaWAN traffic, 

{{< bundle-image alt="The alerting and notification screen on the Multitech Conduit" name="alerting" >}}

Notifications from DeviceHQ are available, however there is no option to customise the alerts you might want to receive, and the only methods of notification are SMS or email.  We'd far prefer to see a set of "standard" alerts that can then be added to, and these days an alerting solution without webhooks makes it very difficult to integrate with "ChatOps" solutions based on slack or even third-party alert providers such as PagerDuty or VictorOps.

The Conduit is a massive step up from the Laird RG1xx, but there are still a significant number of concerns when it comes to monitoring and supporting the device that we'd expect to have been solved given the price tag.


### How easy is it to connect to two popular LoRaWAN services?

As detailed in our [original criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), we're going to test all our gateways based on how well they connect to [The Things Network](https://www.thethingsnetwork.org/) and [ChirpStack](https://chirpstack.io) (upon which our own platform is based).

We'll start with The Things Network, as it's the more popular of the two amongst community networks and the RG1xx definitely falls within the budget of most community groups.

#### Connecting to The Things Network

Like the Laird, the Multitech Conduit comes with presets for The Things Network.

Unlike the Laird, we weren't able to get them working via the web ui, so after more reboots whilst trying those settings out, we reverted to the ["official instructions"](https://www.thethingsnetwork.org/docs/gateways/multitech/).

This involves downloading a script to the device, executing the script, and providing some settings from the console of The Things Network.  

Once we'd done this, the gateway came online and registered correctly, however it does not appear to provide GPS co-ordinates correctly when configured using the script, therefore [TDoA-based location](https://lora-alliance.org/sites/default/files/2018-04/geolocation_whitepaper.pdf) isn't an option as yet.

We're working on getting GPS up and running, and once we do we'll update this post.


#### Connecting to ChirpStack

Connecting to [ChirpStack](https://chirpstack.io) is even more of a challenge, as it requires flashing the firmware from AEP to mLinux in order to install the required software.

Full instructions are available [on the ChirpStack website](https://www.chirpstack.io/gateway-bridge/gateway/multitech/), however as the box we are testing is for a specific client who asked specifically for AEP, we have not had a chance at this time to follow those instructions and see the end results.

### Multitech Conduit Summary

The Multitech Conduit has long been the darling of the "enterprise" LoRaWAN deployment companies, however we've found that it lacks many features that we would expect to find on a premium model.

Whilst the casing is clearly robust, and the need for only one cable makes installing it far more simple than cheaper gateways, we would expect the presets for The Things Network to work without too much effort, and the constant need to reboot in order to apply changes rather than just being able to restart the underlying service makes configuration management far too onerous.

In short, this is a good gateway with a lot of features not seen on the cheaper models, however it is let down by the constant need to restart after every change, poor monitoring support, and an overly arduous initial setup process.
