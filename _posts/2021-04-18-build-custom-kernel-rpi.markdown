---
layout: post
title: "Prepare custom Raspberry PI kernel"
date: 2021-04-19 11:17:00 -0000
categories: IoT Edge
---

Today, I re-configured and build custom Raspberry PI kernel. 

It was needed to enable `CONFIG_BRIDGE_VLAN_FILTERING` feature, to run Moby container.

[Guide]

[Guide]: https://www.raspberrypi.org/documentation/linux/kernel/building.md#choosing_sources
