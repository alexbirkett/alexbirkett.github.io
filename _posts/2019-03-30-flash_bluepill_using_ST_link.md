---
layout: post
title:  "Flashing Blue Pill using ST-LINK V2 (Clone?) / fixing flash: 0"
date:   2019-03-30 09:30 +0100
categories: microcontroller
---

# Programming the Blue Pill using ST-LINK V2

The [STM32F103 "Blue Pill" board](https://www.banggood.com/STM32F103C8T6-Small-System-Board-Microcontroller-STM32-ARM-Core-Board-For-Arduino-p-1058299.html?p=YC161419238657201802) is an inexpensive STM32F103 based microcontroller.

I've previously flashed a USB bootloader to the Blue Pill [using a Raspberry Pi](2018-12-11 flash_blue_pill_using_raspberry_pi.md). The Blue pill's USB connector is

The [ST-LINK V2](https://www.banggood.com/3pcs-3_35V-XTW-ST-LINK-V2-STM8STM32-Simulator-Programmer-Downloader-Debugger-With-20cm-Dupont-Wire-p-1183115.html?p=YC161419238657201802) programmer provides a more reliable way to flash the Blue Pill boards in addition to debugging capabilities.

# Connecting the Blue Pill to the STM-32

| ST-LINK V2 | Blue Pill |
| :---------------------: | :------:|
| PIN 2 SWCLK | SWCLK|
| PIN 4 SWDIO | SWDIO |
| PIN 6 GND   | GND |
| PIN 8 3.3V  | 3.3V |

# Jumper positions
When programming with ST link, it's not necessary to move the jumpers on the Blue Pill board.

# Install Stlink on macOS
`brew install stlink`

`st-info --probe`

```
Found 1 stlink programmers
 serial: 533f6b064975524918500267
openocd: "\x53\x3f\x6b\x06\x49\x75\x52\x49\x18\x50\x02\x67"
  flash: 65536 (pagesize: 1024)
   sram: 20480
 chipid: 0x0410
  descr: F1 Medium-density device
  ```

# Unlock flash (if necessary)

When I ran `st-info --probe`, one of my Blue Pill boards reported reported `flash: 0` This was fixed by [unlocking the flash using OpenOCD](https://github.com/texane/stlink/issues/192).

## Install OpenOCD and telnet
`brew install openocd`

`brew install telenet`

## Run OpenOCD:

In one terminal:

`sudo /usr/local/bin/openocd -f /usr/local/share/openocd/scripts/interface/stlink-v2.cfg -f /usr/local/share/openocd/scripts/target/stm32f1x.cfg`

Leave OpenOCD running and proceed to the next step.

##  Connect with telenet

In a second terminal:

`telnet localhost 4444`

```
Connected to localhost.
Escape character is '^]'.
Open On-Chip Debugger
```
`flash probe 0`

```
device id = 0x20036410
STM32 flash size failed, probe inaccurate - assuming 128k flash
flash size = 128kbytes
flash 'stm32f1x' found at 0x08000000
```

`stm32f1x unlock 0`

```
device id = 0x20036410
STM32 flash size failed, probe inaccurate - assuming 128k flash
flash size = 128kbytes
Device Security Bit Set
stm32f1x.cpu: target state: halted
target halted due to breakpoint, current mode: Thread
xPSR: 0x61000000 pc: 0x2000003a msp: 0x20000400
stm32x unlocked.
INFO: a reset or power cycle is required for the new settings to take effect.
```
# Programming with st-flash

`st-flash --reset write /var/folders/lp/wpsytg2x2mq5qqjjlqm67j1r0000gn/T/arduino_build_430953/sketch_mar23a.ino.bin 0x08000000`

# Links
[Blue Pill on stm32duino.com](https://wiki.stm32duino.com/index.php?title=Blue_Pill)

[stlink issue "Reports 0K of Flash with STM32F205VG"](https://github.com/texane/stlink/issues/192)

[CLion for Embedded Development Part II](https://blog.jetbrains.com/clion/2017/12/clion-for-embedded-development-part-ii/)

[Flashing Bootloaer for Blue Pill boards](https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Flashing-Bootloader-for-BluePill-Boards)

[Reports 0K of Flash](https://github.com/texane/stlink/issues/192)

[stlink / linux mac](https://github.com/texane/stlink)

[stm32duino](http://dan.drown.org/stm32duino/package_STM32duino_index.json)

[STM32F103 Bluepill–Getting Started with Arduino core](https://alselectro.wordpress.com/2018/11/18/stm32f103-bluepill-getting-started-with-arduino-core/)

[STM32F103 BLUEPILL - Getting Started with Arduino Core](https://www.youtube.com/watch?v=zUk0lN1oEwQ)

[Let's start with a Blue Pill](https://jeelabs.org/article/1649a/)

[Blue Pill](https://wiki.stm32duino.com/index.php?title=Blue_Pill)
