---
layout: post
title: "Preparing a custom Raspberry PI kernel"
date: 2021-04-18 11:17:00 -0000
categories: IoT Edge
---

We need to re-configure a Raspberry PI kernel because `CONFIG_BRIDGE_VLAN_FILTERING` configuration is missing. 
It is required to support `Moby`


References
==========

[Building](https://www.raspberrypi.org/documentation/linux/kernel/building.md#choosing_sources)
[Configuring](https://www.raspberrypi.org/documentation/linux/kernel/configuring.md)
