---
layout: post
title:  "Getting started with SPI on Raspberry PI"
date:   2019-01-11 09:30 +0100
categories: raspi
---

[Serial Peripheral Interface (SPI)](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface) synchronous serial communication interface first developed by Motorola in the 1980s.

I want to use some SPI based transceiver modules in future projects e.g. [this CC2500 based module](https://www.aliexpress.com/item/CC2500-PA-LNA-2-4G-SPI-22dBm-Wireless-Data-Transceiver-Module/32606419424.html).


The first step is to get SPI working on the Raspberry Pi.

## Enable SPI
I enabled SPI using the `raspi-config` tool.

From a Raspberry Pi terminal:

`raspi-config`

From the `raspi-config` menu:
* 5 Interfacing Options
* 4 SPI
* Yes (Enable SPI)
* Finish

I rebooted the Pi. (I'm not sure if this is necessary.)

## Testing SPI on the Raspberry Pi

A Raspberry Pi Model B was used. The MOSI pin was connected to the MISO pin according to a [PIN out diagram](https://elinux.org/File:Pi-GPIO-header-26-sm.png) that I found for the Model B:

![Raspberry Pi GPIO header pinout, 26-pin version for older models by Ian Harvey](/assets/images/raspberry_pi_spi_setup/Pi-GPIO-header-26-sm.png)

![MOSI connected to MISO on Rasberry Pi Model B](/assets/images/raspberry_pi_spi_setup/IMG_20190111_090851.jpg)


The [Raspberry Pi SPI documentation](https://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md) describes how to do a loopback test. From a Raspberry Pi terminal:

```
wget https://raw.githubusercontent.com/raspberrypi/linux/rpi-3.10.y/Documentation/spi/spidev_test.c
gcc -o spidev_test spidev_test.c
./spidev_test -D /dev/spidev0.0
```
This is the expected result:
```
spi mode: 0
bits per word: 8
max speed: 500000 Hz (500 KHz)

FF FF FF FF FF FF
40 00 00 00 00 95
FF FF FF FF FF FF
FF FF FF FF FF FF
FF FF FF FF FF FF
DE AD BE EF BA AD
F0 0D
```

If `spidev_test` fails, the output looks like this:

```
spi mode: 0
bits per word: 8
max speed: 500000 Hz (500 KHz)

00 00 00 00 00 00
00 00 00 00 00 00
00 00 00 00 00 00
00 00 00 00 00 00
00 00 00 00 00 00
00 00 00 00 00 00
00 00
```
