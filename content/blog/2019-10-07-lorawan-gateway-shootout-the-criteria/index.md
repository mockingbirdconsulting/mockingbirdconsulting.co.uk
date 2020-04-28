--- 
title: "The LoRaWAN Gateway Shoot-out - the criteria!"
date: 2019-10-04T00:00:00+01:00 
description: "We're taking four of the most popular LoRaWAN gateways and comparing them side by side - here's the criteria..."
featured: true
image: blog/2019-10-07-lorawan-gateway-shootout-the-criteria/gateways.jpg
tags:
  - LoRaWAN 
  - IoT
  - Gateways
resources:
  - name: header
    src: gateways.jpg
  - name: multitech
    src: multitech.jpg
  - name: rak
    src: rak.jpg
  - name: laird
    src: laird.jpg
  - name: imst
    src: imst.jpg

---
The technology that underpins our [farm and estates management solution](https://www.mockingbirdconsulting.co.uk/) is called LoRaWAN.

It enables us to receive and process data about your environment from up to 20 miles away depending on the terrain, and the connection from the sensors back to the analysis and management platform is all provided via a "LoRaWAN Gateway".

The best way to think about the Gateway is like the WiFi Router you have in your house. It might still be the one that your Internet Service Provider gave you, or you might have chosen to purchase your own because you want "more" out of your network.

Just like the WiFi Router market, the LoRaWAN Gateway market is full of promise and wonder, so in this series we're taking four of the most popular LoRaWAN Gateways and putting them to the test.

## What will we cover?

The key things to consider when choosing a new LoRaWAN Gateway are:

   * The environment in which it will live
   * The different types of connectivity available
   * How easy is it to configure?
   * How easy is it to manage once it has been deployed?
   * How easy is it to connect to two popular LoRaWAN services?

Let's look at each of these in turn.

### The environment in which it will live

The majority of our deployments are in rural environments, requiring ruggedised, water-tight cases [rated to IP67 or greater](https://uk.rs-online.com/web/generalDisplay.html?id=ideas-and-advice/ip-ratings) to prevent dust, water, or other things making their way into the enclosure (insects, arachnids, and the like) or destroying the electronics (rats chewing through circuit boards etc., however we have a couple of gateways that are deployed in more "friendly" environments.

{{< pull-quote text="Understanding the environment into which you are going to place your gateway is incredibly important as even an IP68-rated casings will struggle to keep out direct high-pressure jet-washing, yet for an office environment you could happily use a case rated at IP31 as the risk of insects and other contaminants is relatively low.">}}

### The different types of connectivity available

The connection between the sensors and the gateway will always be LoRaWAN in this case, however how do you get the data from the gateway back to the servers for analysis?

As a rule, there are three primary methods of connectivity available to us when sending data from the gateway to the server:

   * LTE (also known as 3G, 4G, or 5G depending on who you speak to)
   * WiFi
   * Ethernet (the same technology used to connect a desktop computer to a corporate network)

As with the casing, understanding the environment in which the device will be placed is vitally important when it comes to connectivity, as your choices may well be limited.

If you can run a network cable out to the proposed site, can you also run power there? [Power over Ethernet](https://en.wikipedia.org/wiki/Power_over_Ethernet) (PoE) can help here, but only if you the gateway at the remote end supports PoE, or you can somehow place the "splitter" in a place it won't be interfered with.

If you can't run cables from the nearest network point (but you can run power), then you're limited to WiFi or LTE - both of which require coverage.  In an urban environment, you're almost certainly going to have a cellular connection, however in rural environments that's far from guaranteed, so you may have to rely on some for of WiFi link.

We favour the Ubiquity Unifi and Edge WiFi equipment (and we're resellers too, so we can provide this as part of an installation), and there are also companies who will connect you to a "rural WiFi" network, but you'll need to know in advance the connectivity options on your access point before you can make an educated selection.

{{< pull-quote text="There is a fourth option that's currently starting to literally take off - Satellite-based LoRaWAN.  This eliminates the need for a gateway as all sensors send their data directly into space, however it comes with the caveat that you can only transmit when the satellite is overhead." >}}

### How easy is it to configure?

Ease of configuration is really important to us.

If we're deploying more than a couple of gateways, we're going to want to do it as fast as we can, and ideally in an automated fashion.

Almost all the gateways on the market come with some kind of web interface to click through and configure, however many of them don't allow for easy automation via SSH or similar, so we'll look at the options for configuration, and how best to at least attempt to automate the solution.

### How easy is it to manage once deployed?

Our engineers come from "Operations" backgrounds rather than development ones, so we're always keen to work out how we're going to monitor and maintain any equipment that we put onto a customer's site, so we know if it fails at 0300hrs.

With each of the gateways, we'll take a look at how easy it is to establish a support connection to the device once it's on site, how easy it is to monitor what the gateway is doing, and the impact it might have on any network charges you might be paying - after all, if the gateway stops working and it's the only one in range, your customers start to lose visibility into their platform, and that's bad for everyone involved.

### How easy is it to connect to two popular LoRaWAN services?

We've looked at most of the commercial offerings out there, and these days unless a customer explicitly requests an alternative then we'll connect them to our own LoRaWAN platform based on [Orne Brocaar's ChirpStack Project](https://www.chirpstack.io/), however we still have a number of customers who are connected to [The Things Network](https://www.thethingsnetwork.org/).

As a result we'll be configuring all of the gateways to talk to each network in turn, and reporting back on how straight-forward the setup was, and whether there were any "glitches" or "gotcha's" along the way.

## Which device is first?

We know that this is a lot of text, but we promise that the next posts in this series will contain images of the devices, their consoles, and any confusing or outstanding things that we find.

The devices we're planning to cover at present are (in the following order!):

   * The Laird Sentrius RG186
   * The Multitech Conduit
   * The [RAK7240 Commercial Gateway](https://www.rakwireless.com/en-us/products/lpwan-gateways-and-concentrators/rak7240)
   * A "home-brew" gateway based on the IMST IC880A and a Raspberry Pi
   
As we discover/purchase/are offered more gateways to review, we'll update the series of posts, but for now we'll leave you with the observation that whilst we'll mention the price of each gateway (including antennae etc.), it's not one of the judging criteria - if a gateway outshines all the others based on functionality, then it's probably worth the money you're paying for it...
