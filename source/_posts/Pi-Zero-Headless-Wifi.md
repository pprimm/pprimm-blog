---
title: Headless Raspberry Pi Zero - Setup Wifi Directly on an SDHC Card on Windows 8
date: 2016-10-03 21:21:57
updated: 2016-10-07 20:05:00
tags:
 - Raspberry Pi Zero
 - Raspbian Jessie Lite
 - IoT
 - Headless Wifi
categories:
 - Tutorial
---
This tutorial is a follow-up (or addendum, if you will) to [_Headless Raspberry Pi Zero Setup with Ethernet from Ground Zero on Windows 8_](/2016/10/02/Pi-Zero-from-Ground-Zero/) which shows how to get a Pi Zero running and connected to a network without using a monitor, keyboard or mouse.  In this follow-up tutorial, we are going to address getting a Pi Zero running with only a Wifi dongle.  This is only ONE way to do this.  Your mileage may vary.

![](/images/Pi-sdhc.jpg)

## Equipment You'll Need
 - Your Windows 8 PC with SD interface or adapter
 - Pi Zero $5 [buy](https://www.adafruit.com/product/2885)
 - Blank 8GB microSDHC w/Adapter $7.50 [buy](https://www.amazon.com/gp/product/B00NICVTA2/)
 - Tiny OTG Adapter $2.95 [buy](https://www.adafruit.com/products/2910)
 - Wifi Dongle [Adafruit $11.95](https://www.adafruit.com/products/814) or [Amazon $9](https://smile.amazon.com/Edimax-EW-7811Un-150Mbps-Raspberry-Supports/dp/B003MTTJOY)
 - 5V 2.4A Switching Power Supply w/ 20AWG 6' MicroUSB Cable $7.95 or equivalent [buy](https://www.adafruit.com/product/1995)
 - _(Optional)_ Adafruit Pi Protector [buy](https://www.adafruit.com/products/2883)

## Software You'll Need
 - Paragon ExtFS for Windows [download](https://www.paragon-software.com/home/extfs-windows-pro/index.html)

## Initial Setup
First, follow steps 1 - 4 in the _Install Raspbian Jessie Lite on 8GB microSDHC Card_ section of my previous [_Headless Raspberry Pi Zero Setup_](/2016/10/02/Pi-Zero-from-Ground-Zero/) post.  Then, come back and perform the steps below for setting the wifi SSID and password.  You can then jump back to the original post and continue with the _Boot the Pi Zero and Connect with PuTTY_ section.

## Setting the Wifi SSID and Password
Once we have the [Raspbian Jessie Lite](https://www.raspberrypi.org/downloads/raspbian/) image on our SDHC card, we can edit the _wpa_supplicant.conf_ file using ExtFS for Windows.  The basic steps for this process were derived the Pi Help topic post: [Setting Wifi Up via the Command Line](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md).  So, follow these easy steps to make the appropriate modification(s) to this file:

1. Download and install [ExtFS for Windows](https://www.paragon-software.com/home/extfs-windows-pro/index.html).  After I installed ExtFS on my Windows 8 box, a driver icon showed up in the system tray.  Although this tool can create and modify partitions (and other things), we're just using it to access the file system and modify a file.

2. With the SDHC Card plugged into your reader, double-click on the ExtFS icon that is in your system tray.  You should see something like this:

   ![](/images/extfs-1.png)

   ExtFS, if properly installed, should recognize the Ext volume and automatically mount as a drive letter in Windows Explorer.  You can select the volume and mount/un-mount.  When you do, it will appear/disappear as a drive letter.  As you can see from the above screen capture, the SDHC card has mounted the Ext volume to drive letter [F:].  The "raw" SDHC card will show up in Explorer as a separate drive letter; mine was drive [E:].  

   **BONUS**: Since we have the SDHC card mounted as a drive, we can take this opportunity to quickly do large file transfers for the Pi to access later. (Tip provided by [Daniel Buentello](https://twitter.com/danielbuentell0))

3. We're now going to modify the file [\etc\wpa_supplicant\wpa_supplicant.conf].  In Windows Explorer, go to the [\etc\wpa_supplicant\] folder and open the [wpa_supplicant.conf] file with an appropriate editor.  I use the [Atom Editor](https://atom.io/).  When I open the file for editing in Atom, the filed contents look like this:

   ```bash
   country=GB
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1
   ```

   I don't care to make any changes here.  We want to go to the bottom of the file (after the `update_config=1` line) and add the following (obviously, modifying it to contain your specific SSID and password):

   ```bash
   network={
       ssid="your-ssid"
       psk="your-password"
   }
   ```

   Now, just save and close the file.

4. Eject the SDHC card from your system.  I didn't have to un-mount the volume in ExtFS before ejecting the SDCH card as this is a setting in ExtFS.  

Now you can pickup with the _Boot the Pi Zero and Connect with PuTTY_ section from the [original post](/2016/10/02/Pi-Zero-from-Ground-Zero/).  Good luck and I hope this helps you get your headless Pi Zero up and running with a Wifi dongle.
