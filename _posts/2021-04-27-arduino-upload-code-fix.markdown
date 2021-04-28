---
layout: post
title: "MXChip: Upload Code MacOSX issue"
date: 2021-04-27 1:27:00 -0000
categories: MXChip
---


`brew tap SomaticLabs/gnu-arm-toolchain`
`brew install gcc-arm-none-eabi openocd`
`cd ~/Library/Arduino15/packages/AZ3166/tools/openocd`
`mv 0.10.0 0.10.0.bak`
`mkdir 0.10.0`
`cd 0.10.0`
`ln -s /usr/local/Cellar/open-ocd/0.10.0 macosx`
`cd macosx`
`ln -s ~/Library/Arduino15/packages/AZ3166/tools/openocd/0.10.0.bak/scripts .`
`cd ~/Library/Arduino15/packages/AZ3166/tools/arm-none-eabi-gcc`
`mv 5_4-2016q3 5_4-2016q3.bak`
`ln -s /usr/local/Cellar/gcc-arm-none-eabi/20160926/ 5_4-2016q3`