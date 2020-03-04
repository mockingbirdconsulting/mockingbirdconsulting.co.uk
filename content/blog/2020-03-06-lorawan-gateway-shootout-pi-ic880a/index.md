--- 
title: "The LoRaWAN Gateway Shoot-out - the IMST IC880a"
date: 2019-10-07T00:00:00+01:00 
description: "How does the IMST IC880a LoRaWAN Gateway stack up against our criteria?"
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

---
The technology that underpins our [farm and estates management solution](https://www.mockingbirdconsulting.co.uk/) is called LoRaWAN.

We're comparing various models of LoRaWAN Gateways to help you work out which is best for your project.

{{< bundle-image alt="The IMST IC880a attached to a RaspberryPi Computer" name="header" >}}

This week, we're looking at the [IMST IC880a](https://www.wireless-solutions.de/products/radiomodules/ic880a.html). Unlike the gateways that we've reviewed up until now, the IMST IC880a is a radio board and needs some additional hardware attached before it can become a "proper" gateway.  

We've included it anyway, because ours has been running for nearly three years without any issues, and it's a great option if you want to build your own gateway or have specific needs that can't be met with pre-packaged software.

We'll be judging it against [this set of criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), so let's see how it compares...

### The environment in which it will live

As the IC880A is a radio board, you can choose the case you place it into, making it incredibly flexible as both an indoor and an outdoor gateway.  We keep our gateway indoors, but run a cable to the outside world for the antenna.

### The different types of connectivity available

The IMST IC880a sits on top of a RaspberryPi, therefore any of the connectivity options available to you via your Pi model are also available to the IMST IC880a. This does mean that whilst the connectivity options are numerous and include LTE (via a USB data device), WiFi, Bluetooth, and Physical Ethernet, Power over Ethernet is not an option without additional hardware.

### How easy is it to configure?

Configuration of the IMST IC880a is done via the packet forwarder installed on the 

### How easy is it to manage once deployed?

Once deployed, the IMST IC880a tends to "just work", however if you need to perform any maintenance then you'll need to be on the same network as the gateway as it lacks any kind of "remote access" functionality.

Monitoring is similarly missing from the gateway, meaning that you need to track the traffic arriving from the gateway as a "heartbeat" and alert if data is not seen within a particular time frame.

All in all, although this is one of the easiest gateways to configure, it's also one of the most difficult to manage without physical access to either the network it is attached to or the device itself.

### How easy is it to connect to two popular LoRaWAN services?

As detailed in our [original criteria](/blog/2019-10-07-lorawan-gateway-shootout-the-criteria/), we're going to test all our gateways based on how well they connect to [The Things Network](https://www.thethingsnetwork.org/) and [ChirpStack](https://chirpstack.io) (upon which our own platform is based).

We'll start with The Things Network, as it's the more popular of the two amongst community networks and the IMST IC880a definitely falls within the budget of most community groups.

#### Connecting to The Things Network

Connecting to "The Things Network" with the IMST IC880a is trivial.

Go to the LoRaWAN Settings, select "The Things Network - EU" from the drop-down list in the "presets" section, and then follow the quickstart guide in the IMST IC880a manual.  You'll be up and running in under 5 minutes assuming you can click the mouse fast enough!

{{< bundle-image alt="The preset options for the IMST IC880a" name="presets" >}}

You'll want to select "The Things Network - EU" unless you either have a comprehensive understanding of what the "Legacy" packet forwarder approach means or have been told to do so by someone trying to provide you with support on either the forums or the slack channels.

Once the connection has been made, the Dashboard will show that the gateway is connected to The Things Network, and the status indicator will go green.

#### Connecting to ChirpStack

Connecting to ChirpStack is a little bit more involved if you are running an up-to-date installation (i.e. v3 or above), as the firmware on the IMST IC880a only supports version 2.x of ChirpStack.

Full instructions on how to configure the IMST IC880a to talk to v2 of the ChirpStack are [available on the website](https://www.chirpstack.io/lora-gateway-bridge/gateway/laird/) and will take a similar amount of time as connecting to The Things Network, however for version 3 of ChirpStack you'll need to do the following:

   1. [Follow the instructions](https://www.chirpstack.io/lora-gateway-bridge/gateway/laird/#semtech-forwarder) on the ChirpStack website to configure the Semtech Forwarder
   2. Install [LoRa Gateway Bridge](https://www.chirpstack.io/lora-gateway-bridge/install/) onto either a server or another device on the same network as the gateway
   3. Configure the Semtech Forwarder to point to the LoRa Gateway Bridge that you've just setup, and then configure the LoRa Gateway Bridge to connect to your ChirpStack

In the case of v3, you won't see the status indicator on the dashboard change colour, so the only way to confirm that the gateway is connected is to log into the console and check the last time it reported in.

{{< bundle-image alt="A working gateway in the ChirpStack Dashboard" name="status" >}}


### RG186 Summary

The RG186 is a great gateway for those starting out on their LoRaWAN journey or who are able to have their management devices on the same network as the gateway.  It's a relatively cheap, no-frills gateway that lacks many of the features we'll see in other posts in this series, but most of the time you won't need them unless you're working at "enterprise" levels.

We'd love to see VPN support built in to the firmware, along with some better monitoring (even a `/health` http endpoint which returns the gateway connections status that we can hit from one of our existing monitoring tools would be a massive improvement!), PoE would also be great, and LTE support would be the icing on the cake, but we're also realistic that we can't expect that for a device that costs £200.

All in all, if you want something a bit more robust than your home-brew DIY gateway (more on those in a future post!) then this is a great option, but if you're deploying more than 5 of them or you don't have access to the site, you'll probably struggle to get the functionality you need.
