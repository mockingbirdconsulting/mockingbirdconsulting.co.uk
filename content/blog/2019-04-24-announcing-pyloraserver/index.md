---
title: Announcing "PyLoRaServer"
author: Mockingbird Consulting
type: post
date: 2019-04-24T06:00:00+00:00
description: "A python library for working with LoRaServer.io"
resources:
  - name: header
    src: pyloraservercode.png
tags:
  - Internet of Things
  - iot
  - lorawan
  - loraserver.io
---
The [LoRaServer.io](https://loraserver.io) project powers our platforms that [deliver value to our customers](/case_studies) and underpins our [shared data collection network](https://shared.ourdata.network/).

Today we're announcing the launch of a new project: [PyLoRaServer](https://github.com/mockingbirdconsulting/pyloraserver).

PyLoRaServer is a python library designed to help developers interact with the [LoRaServer.io](https://loraserver.io) project from within their existing python applications.

This could range from providing additional functionality to an existing portal through to using [TensorFlow](https://www.tensorflow.org/) to analyse data streams and trigger LoRaWAN events and alerts based on metric thresholds.

As well as releasing the code, we're also publishing the documentation on [Read The Docs](pyloraserver.readthedocs.io) where you'll find examples on how to list and create new devices alongside full descriptions of each class and the associated methods.

{{< bundle-image name="header" caption="The PyLoRaServer code in action" >}}

We're already using the library to provision new devices, and as we improve the functionality, we'll be baking it in to our products as well, but we don't just want it to be us who uses and benefits from this.

We've released the code under the [MIT License](https://tldrlegal.com/license/mit-license), allowing you to take the code, use it, and hopefully contribute back to the project.

If there is functionality that you wish to see, if you've found a bug, or if you just want to improve the quality of the code, please come and [join us on Github](https://github.com/mockingbirdconsulting/pyloraserver/) - we'd love to work with you.
