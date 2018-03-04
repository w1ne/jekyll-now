---
layout: post
title: How to make Linux work with USB-to-serial
blogid: personal
---

I am using [SourceRabbit G-code sender](https://www.sourcerabbit.com/) to control my "Miyazaki" drawing bot.
Drawing bot is using [CP2102](https://www.silabs.com/products/interface/usb-bridges/classic-usb-bridges/device.cp2102) chip as a Virtual com port bridge between USB and control application.
Unfortunately it isn't working out of the box in OpenSuse. 
USB-to-serial converter is mounted as `/dev/ttyUSB0`, with following rights:

```
ls -la /dev/ttyUSB0
crw-rw---- 1 root dialout 188, 0 Mar  4 20:35 /dev/ttyUSB0
```
Device is owned by root and read and write permissions are granted for the `dialout` group.
The solution is add your user to correct groups:
1. Run in terminal `sudo usermod -a -G dialout %username%`.
*Current user groups can be checked with `groups` command.*
2. Log off and login again to apply group changes. 
3. Run application which is using USB-to-serial adapter, e.g. `java -jar "SourceRabbit-GCODE-Sender.jar"`
4. SourceRabbit G-code Sender is now working with CP2102 adapter.
![SourceRabbit G-code Sender]({{BASE_PATH}}/images/img/04-03-2018/SourceRabbit-G-code-Sender.png)
