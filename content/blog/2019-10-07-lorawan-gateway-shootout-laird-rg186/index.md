--- 
title: "The LoRaWAN Gateway Shoot-out - the Laird RG186"
date: 2019-10-07T00:00:00+01:00 
description: "How does the Laird RG186 LoRaWAN Gateway stack up against our criteria?"
featured: true
image: blog/2019-10-07-lorawan-gateway-shootout-laird-rg186/LoRaGateway01.png
tags:
  - LoRaWAN 
  - IoT
  - Gateways
  - Laird
resources:
  - name: header
    src: laird.jpg
  - name: presets
    src: presets.png
  - name: status
    src: gatewaystatus.png
reviewOverview: A budget, indoor gateway with limited management features, but perfect for small projects or experimentation.
keyFacts:
  positive:
    - Easy to configure
    - WiFi connectivity
  negative:
    - No LTE support
    - No remote management capability or VPN Configuration

---
The technology that underpins our [farm and estates management solution](https://www.mockingbirdconsulting.co.uk/) is called LoRaWAN.

We're comparing various models of LoRaWAN Gateways to help you work out which is best for your project.

This week, we're looking at the [Laird RG186](https://www.lairdconnect.com/wireless-modules/lorawan-solutions/sentrius-rg1xx-lora-enabled-gateway-wi-fi-bluetooth-ethernet) - an indoor gateway that retails for around £200 and is available from the larger hardware suppliers such as RS Components and Mouser.

We'll be judging it against [this set of criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), so let's see how it compares...

### The environment in which it will live

The RG186 is designed to be an indoor gateway, and by default comes in a moulded plastic housing without a listed IP rating.

There is also now the IP67-rated IP67-RG1xx line of gateways which contain the same hardware in an "official" outdoor casing, meaning that you don't need to [source your own](https://www.thethingsnetwork.org/forum/t/laird-rg191-working-in-australia/10044/31) any more, however as it's exactly the same hardware just in a different case, the rest of the comparisons should apply to both models.

The lack of an IP-rated case for our model means that it's not suitable for outdoor use, however we've had successful data transmissions of over 5 miles across a mixed urban/rural environment to the Laird RG186 in the roof-space of a building so we're confident that if you're equipping an office building or buildings on a country estate to monitor the interior and close surrounding areas, this is a good option.

You *could* add an external antenna of some kind for the LoRaWAN connection and run a cable through a wall/roof to provide a better signal, but this is untested by us and we therefore can't recommend it.

### The different types of connectivity available

The RG1xx series has three methods to connect to it - ethernet, WiFi, or Bluetooth.  Bluetooth is only really suitable for connecting to configure the device, so we'll focus on the other two options.

#### Ethernet (standard networking)

The ethernet port is situated on the front of the unit, opposite the various antenna, and next to the power supply input.  When you place the gateway on a shelf this feels a bit counter-intuitive when running cables, however as soon as you mount the unit on the wall, the "front" becomes the "bottom", with the antenna at the "back" becoming the "top" of the unit which makes a great deal more sense.

Unlike some of the other units we'll be looking at through this series, there is no support for Power over Ethernet (PoE), meaning that unless you purchase a PoE adaptor kit, you'll need to have a power socket within about 1m of the gateway.  Ethernet specifications mean that the network port for the other end of the cable can be up to 100m away, so as long as you're willing to drill holes to run the network cable there's plenty of scope for where you position this gateway.

#### WiFi

The WiFi options on this gateway mean that as long as you are within range of a WiFi router or access point, you can get an internet connection to it.  This includes the "long-distance" WiFi equipment designed for rural links, enabling you to place the gateway up to 150m away from your internet connection as long as you can run power to that location.

The WiFi setup is reasonably trivial to configure (more on this later), however if you want to add a network that the device can't auto-discover (for example when configuring in the office before sending to site), you'll need to go into the "advanced" settings.

#### Conclusion

The options to either hard-wire or connect via WiFi make this unit reasonably flexible, however the short power lead, lack of LTE functionality, and lack of PoE (whilst expected given the low price) could affect placement and therefore LoRaWAN range of the device.

### How easy is it to configure?

The RG1xx range of gateways are only configurable via their web interface.  This is fine when you're installing/configuring one or two, however (as we'll see in future posts) it rapidly becomes problematic once you get about 5 or so gateways.

Having said that, the User Interface is relatively intuitive and has a limited set of options, so it's quite quick to get the device connected to WiFi and your preferred LoRaWAN supplier either via one of the presets or using an intermediary device to forward the data on (see the section on connecting to [The Things Network](https://www.thethingsnetwork.org/) and [ChirpStack](https://chirpstack.io) below for more details on this).

### How easy is it to manage once deployed?

Once deployed, the Laird RG1xx tends to "just work", however if you need to perform any maintenance then you'll need to be on the same network as the gateway as it lacks any kind of "remote access" functionality.

Monitoring is similarly missing from the gateway, meaning that you need to track the traffic arriving from the gateway as a "heartbeat" and alert if data is not seen within a particular time frame.

All in all, although this is one of the easiest gateways to configure, it's also one of the most difficult to manage without physical access to either the network it is attached to or the device itself.

### How easy is it to connect to two popular LoRaWAN services?

As detailed in our [original criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), we're going to test all our gateways based on how well they connect to [The Things Network](https://www.thethingsnetwork.org/) and [ChirpStack](https://chirpstack.io) (upon which our own platform is based).

We'll start with The Things Network, as it's the more popular of the two amongst community networks and the RG1xx definitely falls within the budget of most community groups.

#### Connecting to The Things Network

Connecting to "The Things Network" with the RG1xx is trivial.

Go to the LoRaWAN Settings, select "The Things Network - EU" from the drop-down list in the "presets" section, and then follow the quickstart guide in the RG1xx manual.  You'll be up and running in under 5 minutes assuming you can click the mouse fast enough!

{{< bundle-image alt="The preset options for the RG1xx" name="presets" >}}

You'll want to select "The Things Network - EU" unless you either have a comprehensive understanding of what the "Legacy" packet forwarder approach means or have been told to do so by someone trying to provide you with support on either the forums or the slack channels.

Once the connection has been made, the Dashboard will show that the gateway is connected to The Things Network, and the status indicator will go green.

#### Connecting to ChirpStack

Connecting to ChirpStack is a little bit more involved if you are running an up-to-date installation (i.e. v3 or above), as the firmware on the RG1xx only supports version 2.x of ChirpStack.

Full instructions on how to configure the Laird RG1xx to talk to v2 of the ChirpStack are [available on the website](https://www.chirpstack.io/lora-gateway-bridge/gateway/laird/) and will take a similar amount of time as connecting to The Things Network, however for version 3 of ChirpStack you'll need to do the following:

   1. [Follow the instructions](https://www.chirpstack.io/lora-gateway-bridge/gateway/laird/#semtech-forwarder) on the ChirpStack website to configure the Semtech Forwarder
   2. Install [LoRa Gateway Bridge](https://www.chirpstack.io/lora-gateway-bridge/install/) onto either a server or another device on the same network as the gateway
   3. Configure the Semtech Forwarder to point to the LoRa Gateway Bridge that you've just setup, and then configure the LoRa Gateway Bridge to connect to your ChirpStack

In the case of v3, you won't see the status indicator on the dashboard change colour, so the only way to confirm that the gateway is connected is to log into the console and check the last time it reported in.

{{< bundle-image alt="A working gateway in the ChirpStack Dashboard" name="status" >}}


### RG186 Summary

The RG186 is a great gateway for those starting out on their LoRaWAN journey or who are able to have their management devices on the same network as the gateway.  It's a relatively cheap, no-frills gateway that lacks many of the features we'll see in other posts in this series, but most of the time you won't need them unless you're working at "enterprise" levels.

We'd love to see VPN support built in to the firmware, along with some better monitoring (even a `/health` http endpoint which returns the gateway connections status that we can hit from one of our existing monitoring tools would be a massive improvement!), PoE would also be great, and LTE support would be the icing on the cake, but we're also realistic that we can't expect that for a device that costs £200.

All in all, if you want something a bit more robust than your home-brew DIY gateway (more on those in a future post!) then this is a great option, but if you're deploying more than 5 of them or you don't have access to the site, you'll probably struggle to get the functionality you need.
