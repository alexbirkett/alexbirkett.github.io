---
layout: post
title:  "Why you may no longer need the more expensive and heavier version of the Mobula7 in the EU"
date:   2019-01-06 20:00 +0100
categories: quad
---

I [posted]({% post_url 2019-01-05-mobula_7_EU %}) yesterday about why the EU-LBT version of the [Happymodel Mobula7](
https://www.banggood.com/Happymodel-Mobula7-75mm-Crazybee-F3-Pro-OSD-2S-Whoop-FPV-Racing-Drone-w-700TVL-Camera-BNF-p-1357971.html?p=YC161419238657201802) includes an external [Frsky XM+ receiver]( https://www.banggood.com/Frsky-XM-Micro-D16-SBUS-Full-Range-Receiver-Up-to-16CH-p-1110020.html?p=YC161419238657201802) to comply the EU-LBT (listen before talk) regulations. The external receiver adds weight, drag and cost to the EU version of the Mobula7.

The FrSky version of the [Racerstar Crazybee F3 Pro Flight Controller](https://www.banggood.com/Racerstar-Crazybee-F3-Pro-Flight-Controller-Mobula7-5A-1-2S-Compatible-Flysky-or-Frsky-or-DSMX-Receiver-p-1361634.html?p=YC161419238657201802) used on the Mobula7 has a [TI CC2500 2.4 GHz RF Transceiver](http://www.ti.com/product/CC2500) soldered directly on to it. This can be used to receive the signals for the non-EU version of FrSky transmitters but the not the EU version because of missing software support ... until now!

A [pull request](https://github.com/betaflight/betaflight/pull/7339) has been sent to Betaflight to add support for the EU version of the FrSky protocol.

This means that the external [Frsky XM+ receiver]( https://www.banggood.com/Frsky-XM-Micro-D16-SBUS-Full-Range-Receiver-Up-to-16CH-p-1110020.html?p=YC161419238657201802) may no longer necessary. There is some debate on the pull request about whether the implementation actual complies with the EU regulations but it I made it work with the EU version of the [Taranis Q X7 transmitter](https://www.banggood.com/no/FrSky-ACCST-Taranis-Q-X7-2_4GHz-16CH-Transmitter-White-Black-p-1112717.html?p=YC161419238657201802) running the EU version of the transmitter firmware (`XJT_EU170317.frk`)

# Building the code
I built phobos's `frsky-x-lbt` pull request branch for the `CRAZYBEEF3FR` target.

In the terminal on a Mac:
```
git clone git@github.com:phobos-/betaflight.git
cd betaflight
git checkout frsky-x-lbt
make arm_sdk_install
make TARGET=CRAZYBEEF3FR
```

This creates a HEX in  [`./obj/betaflight_4.0.0_CRAZYBEEF3FR.hex`](/assets/binary/betaflight_4.0.0_CRAZYBEEF3FR.hex) that can be flashed onto the Mobula7's flight controller using the Betaflight configurator.

# Flashing the custom firmware

## AT YOUR OWN RISK

I have no idea how stable this software is. It's in an unknown state between Betaflight releases. So far I'm only done a test hover.

* Connect the Mobula7 to your computer with a USB cable.

In the Betaflight configurator:

* Click the "Firmware flasher" tab.
* Click "Load Firmware [Local]" button
* Select the firmware you built (or [downloaded](/assets/binary/betaflight_4.0.0_CRAZYBEEF3FR.hex))
* Click "Flash firmware"

# Configuring FrSky EU-LBT
The EU version of the FrSky D16 protocol is known as FRSKY_X_LBT. At the time of writing there is no way to configure it using the Betaflight configurator GUI but it can be setup from the "CLI tab".

On the "Configuration" tab, set non-EU FRSKY_X protocol:

![FRSKY_X](/assets/images/mobula7_EU/SPI_FRSKY_X.png)

And then from the "CLI" tab type:

`set rx_spi_protocol = FRSKY_X_LBT`

and then

`save`

# Binding transmitter

Bind the transmitter on the flight controller, not on the external Frsky XM+ receiver.

To put the flight controller into bind mode, hold down the bind button for two seconds.

There are [binding instructions](https://www.youtube.com/watch?v=z7lHumHW0UE) on Youtube.

# Restoring factory Firmware

My Mobula7 came with [firmware](http://www.happymodel.cn/index.php/2018/12/28/betaflight-4-0-customized-firmware-for-mobula7/) that appears to be built from git hash `3b479f92d` which is not an official release:

`# Betaflight / CRAZYBEEF3FR (CBFR) 4.0.0 Oct 13 2018 / 08:00:14 (3b479f92d) MSP API: 1.41`
