--- 
title: "The LoRaWAN Gateway Shoot-out - the RAK 7240"
date: 2019-12-17T00:00:00+00:00 
description: "How does the RAK 7240 LoRaWAN Gateway stack up against our criteria?"
summary: >
    This week, we're looking at the RAK 7240 - an outdoor gateway that retails for around £500 and is available via RAK resellers.
featured: true
image: blog/2019-12-17-lorawan-gateway-shootout-rak-7240/rak.jpg
images:
  - blog/2019-12-17-lorawan-gateway-shootout-rak-7240/rak.jpg
tags:
  - LoRaWAN 
  - IoT
  - Gateways
  - RAK
  - Gateway Reviews
resources:
  - name: header
    src: rak.jpg
  - name: presets
    src: presets.png
  - name: status
    src: gatewaystatus.png
reviewOverview: A fantastic router for the price, easy to setup and clear on current activity.
keyFacts:
  positive:
    - Easy to configure
    - Fantastic overview screen
  negative:
    - Installing custom software is unsupported
learnMore:
  - name: The Criteria
    uri: /blog/2019-10-07-lorawan-gateway-shootout-the-criteria/
  - name: Other Reviews
    uri: /blog/2019-10-07-lorawan-gateway-shootout-the-criteria/
---
The technology that underpins our [farm and estates management solution](https://www.mockingbirdconsulting.co.uk/) is called LoRaWAN.

We're comparing various models of LoRaWAN Gateways to help you work out which is best for your project.

This week, we're looking at the [RAK 7240](https://www.rakwireless.com/en-us/products/lpwan-gateways-and-concentrators/rak7240) - an outdoor gateway that retails for around £500 and is available via RAK resellers.

Power is *only* supplied via Power over Ethernet (PoE), so an ethernet cable will still need to be run and a PoE injector needs to be present in the system, however the PoE option means that unlike the [Laird RG1xx we reviewed previously](/blog/2019-10-07-lorawan-gateway-shootout-laird-rg186/) you can connect these gateways to the internet with just a single cable.

The RAK7240 also has GPS built in with a dedicated external antennae, making it possible to use triangulation between multiple gateways to remove the need for GPS on devices and use [Time Difference of Arrival](https://lora-alliance.org/sites/default/files/2018-04/geolocation_whitepaper.pdf) between gateways to locate a sensor.

Results on TDOA from others we know who are using this in practice appear to be mixed, however the GPS on the gateway allows us to be confident that the clock is set correctly and that it is showing in the right location on the gateway maps on the various platforms we use.

We'll be judging it against [this set of criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), so let's see how it compares...

### The environment in which it will live

The RAK 7240 is designed to be an outdoor gateway, and comes in a metal housing with an official rating of IP-67.

This casing makes it perfect for outdoor or other challenging environments such as dusty factory floors and rooms that are subject to high humidity.

### The different types of connectivity available

The unlike the [Multitech Conduit](/blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/), the RAK7240 comes with WiFi, LTE, and Ethernet (over the previously mentioned PoE port) by default, making it a great choice when you're not quite sure what the connectivity looks like at the remote site.

#### Ethernet (standard networking)

The ethernet port is situated on the underside of the unit within an IP-67 rated socket, providing easy routing of cables down from any mounting point.  As with the [Multitech Conduit](/blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/) the 10/100Mbps is slower than most current networking hardware, however unless you have hundreds or thousands of devices sending data via the same gateway, it is unlikely this will cause a bottleneck.

The ethernet port also provides 802.11af compatible Power over Ethernet to supply power to the device, meaning that you need a PoE injector.  The good news is that if you have a PoE switch already in place (and most enterprise organisations will have this these days), you can plug a single cable between the gateway and your switch and have that one cable carry both data *and* power.

The ethernet port forms a central part of the initial setup so you'll need a PoE injector just to get up and running with this one, however unlike the [Multitech Conduit](/blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/) DHCP is enabled by default, so the device will automatically obtain an IP Address. 

Check your router for the IP Address that has been assigned, visit it in your browser, and log in.  The default credentials are `root:root`.

#### LTE (4G Cellular)

As the model we are using will end up on our demo farm, we're checking that relying on LTE for our alternative connectivity back to the LoRaWAN servers will work for all our other customers.  This requires a SIM card from a mobile provider (not supplied), and therefore an additional monthly cost.  We're using [1p mobile](https://www.1pmobile.com/) and with the 10 or so sensors that are constantly sending back data in our lab environment we're seeing about 16Mb/day in data transfer.  With this particular provider, that means and additional cost of less than £6.00 GBP each month!  As the amount of data that we send over LTE increases, we'll probably need to look to a monthly "all you can eat" contract, but for the time being (and for smaller farms/estates) this will suffice.

The gateway is also not configured to connect to any particular network as standard, so you'll need to find the APN and other data settings for your mobile provider in order to connect via LTE.

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

Monitoring of the RAK 7240 is relatively straight forward, and it has an excellent status page that gives an overview of all the connectivity types, the number of devices attached, LoRaWAN packets sent. 

{{< bundle-image alt="The RAK7240 Gateway Status Page" name="status" caption="The RAK 7240 Gateway Status Page" >}}

We already have an MQTT connection from the gateway for our LoRaWAN traffic and our aim in future is to utilise this to send more advanced metrics back to our monitoring platform, however for now we can use both the TTN NOC API (be careful, this is not stable as using it for monitoring is not really its intended purpose!), or the Chipstack API to see when the gateway last reported in to the appropriate platform.

Pulling this data into Grafana means that we can easily integrate with "ChatOps" solutions based on slack and third-party alert providers such as PagerDuty or VictorOps, allowing us to alert when a gateway has been offline or hasn't seen any data for a given amount of time.

All in all, the ease of configuration, options for future monitoring tooling, and stability of connectivity failover make this the best gateway we've used so far when it comes to supporting and deploying the device.

### How easy is it to connect to two popular LoRaWAN services?

As detailed in our [original criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), we're going to test all our gateways based on how well they connect to [The Things Network](https://www.thethingsnetwork.org/) and [ChirpStack](https://chirpstack.io) (upon which our own platform is based).

We'll start with The Things Network, as it's the more popular of the two amongst community networks and the RAK 7240 is at the price point where it's just cheap enough to justify for self-funded village or town deployments.

#### Connecting to The Things Network

Unlike the Laird or the Multitech, the RAK comes with The Things Network configured by default - you just need to set your Gateway EUI, update the Server URL to point at your local router, and you should see data start to appear.

GPS appears to work well without any additional effort required, and there's not much more we can say.  Once again, RAK have provided the most user-friendly and efficient setup so far!


#### Connecting to ChirpStack

Connecting to [ChirpStack](https://chirpstack.io) is also trivial.  The gateway comes with support for Chirpstack 2 and 3, a built in packet forwarder, and an easy way to configure all the MQTT settings required to get the data back to your installation.

All we needed to do was select the correct version from the dropdown, set the EUI, and enter our MQTT settings and the whole thing sprang to life.

### RAK 7240 Summary

Whilst the lack of general availability via the web store and the need to go via the RAK Reseller programme is a bit of a pain, we're really struggling to find any other issues with the RAK 7240 LoRaWAN Gateway.

The price is excellent value for the quality of the device, the setup and configuration process is trivial, and the feature-set is far greater than the Multitech at half the price!

This gateway will be going onto our Demonstration Farm to prove itself very soon, however at the time of writing this post the RAK 7240 is so impressive that we'll be basing all future rollouts on it until something better or more appropriate to the specific project environment comes along.
