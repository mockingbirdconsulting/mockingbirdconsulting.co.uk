--- 
title: "The LoRaWAN Gateway Shoot-out - the Multitech Conduit"
date: 2019-11-06T00:00:00+00:00 
description: "How does the Multitech Conduit LoRaWAN Gateway stack up against our criteria?"
summary: >
    This week, we're looking at the well respected Multitech Conduit - an outdoor gateway that retails for around £1,000 and available from a number of hardware suppliers including RS Components.
featured: true
image: blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/MultitechConduit.png
images: 
  - blog/2019-11-06-lorawan-gateway-shootout-multitech-conduit/MultitechConduit.png
tags:
  - LoRaWAN 
  - IoT
  - Gateways
  - Multitech
resources:
  - name: header
    src: MultitechConduit.png
  - name: presets
    src: presets.png
  - name: status
    src: gatewaystatus.png
  - name: alerting
    src: DeviceHQAlerts.png
reviewOverview: Proven technology forming the backbone of many large-scale deployments, but falls short when it comes to ease of configuration and monitoring.
keyFacts:
  positive:
    - Robust Casing
    - Proven Technology from an established vendor
    - A popular choice for enterprise deployments, so there's a lot of support out there if needed
  negative:
    - Initial setup involves too much effort with network addresses
    - Expensive compared to similar offerings on the market
    - Alerting solution could do with an update to using Webhooks rather than just SMS/Email
---
The technology that underpins our [farm and estates management solution](https://www.mockingbirdconsulting.co.uk/) is called LoRaWAN.

We're comparing various models of LoRaWAN Gateways to help you work out which is best for your project.

This week, we're looking at the well respected [Multitech Conduit](https://www.multitech.com/brands/multiconnect-conduit-ip67) - an outdoor gateway that retails for around £1,000 and available from a number of hardware suppliers including RS Components.

Power is *only* supplied via Power over Ethernet (PoE), so an ethernet cable will still need to be run and a PoE injector needs to be present in the system, however the PoE option means that unlike the [Laird RG1xx we reviewed previously](/blog/2019-10-07-lorawan-gateway-shootout-laird-rg186/) you can connect these gateways to the internet with just a single cable.

The Conduit also has GPS built in, making it possible to use triangulation between multiple gateways to remove the need for GPS on devices and use [Time Difference of Arrival](https://lora-alliance.org/sites/default/files/2018-04/geolocation_whitepaper.pdf) between gateways to locate a sensor.

Results on this from others we know who are using this in practice appear to be mixed, however the GPS on the gateway allows us to be confident that the clock is set correctly and that it is showing in the right location on the gateway maps on the various platforms we use.

We'll be judging it against [this set of criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), so let's see how it compares...

### The environment in which it will live

The Conduit is designed to be an out gateway, and comes in a metal housing with an official rating of IP-67.

This casing makes it perfect for outdoor or other challenging environments such as dusty factory floors and rooms that are subject to high humidity.

### The different types of connectivity available

The Conduit series has multiple methods of connectivity available depending on the model.  The ones we're working with have Ethernet or LTE (4G Cellular), however you can also purchase versions that add WiFi.

The lack of WiFi as a "default" option is a bit of a pain, however as we've paid extra for the cellular installation we still have connectivity options that don't rely on the gateway needing to be within 100m of an internet connection.


#### Ethernet (standard networking)

The ethernet port is situated on the underside of the unit within an IP-67 rated socket, providing easy routing of cables down from any mounting point.  The 10/100Mbps is slower than most current networking hardware, however unless you have hundreds or thousands of devices sending data via the same gateway, it is unlikely this will cause a bottleneck.

The ethernet port also provides 802.11af compatible Power over Ethernet to supply power to the device, meaning that you need a PoE injector.  The good news is that if you have a PoE switch already in place (and most enterprise organisations will have this these days), you can plug a single cable between the gateway and your switch and have that one cable carry both data *and* power.

The ethernet port forms a central part of the initial setup so you'll need a PoE injector just to get up and running with this one, and the default setting to **not** automatically get an IP Address from your local network adds an extra 5 minutes or so to the setup process whilst you reconfigure your computer to connect to the device, however after that it provides an incredibly reliable connection and can be reconfigured to get an IP address from a central server or router in line with your network policies.

{{< pull-quote text="When you first connect the Conduit to your laptop, it will have an IP address of 192.168.2.1, and no DHCP server enabled.  This means you need to manually configure your laptop to have an address in the 192.168.2.0/24 range, and also means that if you connect the conduit to a network that already uses that range, it will probably take the network offline whilst everything on the network fights about whether the conduit should have that IP address or not." >}}

#### LTE (4G Cellular)

As the model we are using does not have WiFi, we're relying on LTE for our alternative connectivity back to the LoRaWAN servers.  This requires a SIM card from a mobile provider (not supplied), and therefore an additional monthly cost.  The gateway is also not configured to connect to any particular network as standard, so you'll need to find the APN and other data settings for your mobile provider in order to connect via LTE.

So far, we've found the LTE to be just as reliable as the ethernet, and the failover between the two connections when we disconnect the network supply from the PoE injector is flawless.

#### Conclusion

The options to either hard-wire or connect via LTE make this unit highly flexible, however the lack of WiFi as standard is a bit of a pain for sites where network sockets are few and far between.

The LTE failover is a massive plus however, and we'll be shipping all of our gateways with SIM cards installed and enabled so that in the event of a network outage the customer data continues to be collected.

### How easy is it to configure?

The Multitech Conduit comes in two versions - mLinux (which runs a heavily customised version of the Linux operating system, using opkg as the package manager to install new things), and the "AEP" version which has a built in web-ui as well as a dedicated LoRaWAN Server.  We've got the AEP version, so we'll focus on that here.

#### Initial setup

As mentioned above, the setup process isn't quite as straight-forward as we'd like, and follows these steps:

   1. Connect the Conduit to a computer via a PoE Injector
   2. Configure the ethernet interface on that computer to listen on 192.168.2.x
   3. Access the Conduit's web interface via http://192.168.2.1/
   4. Set the initial "admin" password
   5. Log in and start to configure the device

The first thing we did was configure the gateway to use DHCP so we could attach it to our test network instead of directly to one of our laptops.  This was the first time we discovered that for **every single change** you have to save and restart the device, with a restart taking anywhere up to **5 minutes**.

Once we had access to it via the test network, the next step was to log back in again and start to explore the various menus.  We had hoped that trivial things such as setting the hostname or connecting to a LoRaWAN provider would be a couple of clicks in the web interface, however this proved not to be the case.

Setting many of the features that we would consider as "standard" appear to be only possible from the command line.  Thankfully you can enable SSH and log directly into the box, and as this appears to be a requirement for connecting to [The Things Network](https://www.thethingsnetwork.org), we enabled it for the LAN interface and then clicked "save and restart" (another 5 minute wait).

In total, by the time we'd configured the "system alias", disabled the on-board node-red server, configured DHCP in client mode (you can also have the Multitech act as a DHCP server, however we struggled to think of a scenario when you'd want this), enabled access via SSH, and then saved/restarted to apply these changes, it had taken nearly 30 minutes to get to a point where we could configure it for a LoRaWAN provider - way above what we'd expect for a device that costs this amount of money.

### How easy is it to manage once deployed?

The Multitech Conduit runs a custom flavour of linux, and comes with support for OpenVPN built in which is a marked improvement over the [Laird RG1xx we reviewed previously](/blog/2019-10-07-lorawan-gateway-shootout-laird-rg186/), however as we use both certificate and username/password authentication for our support VPN configuring the connection required the extra step of adding a file with the credentials to the device.  As with all changes, this then required a "save and restart" each time we needed to amend the config.

Once the VPN is configured, a test of failover between the LTE and Ethernet connections revealed a very quick switchover and reconnect, providing significant reassurance that once installed these devices will continue to "phone home" to our support server and seamlessly switch between the primary and backup connectivity options.

Monitoring of the Multitech Conduits is meant to be done via the [Multitech "Device HQ"](https://www.multitech.com/brands/devicehq), and configuration of this is relatively straight forward once you are registered on the portal, however it does require "save and restart" again, and there doesn't appear to be a way of rotating the API key you are given without contacting Multitech Support.

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
