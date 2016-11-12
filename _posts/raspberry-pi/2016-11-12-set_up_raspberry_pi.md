---
title: Set up Raspberry Pi  
category: raspberry pi  
tags: [raspberry pi]  
layout: post  
lang: en
---

### Installation

#### A. Use NOOBS (New Out Of the Box Software)

The step are copied from [https://www.raspberrypi.org/learning/software-guide/quickstart/](https://www.raspberrypi.org/learning/software-guide/quickstart/)

1. Visit the [SD Association’s website](http://www.sdcard.org/) and download [SD Formatter 4.0](https://www.sdcard.org/downloads/formatter_4/index.html) for either Windows or Mac.

    Or use terminal,
    `curl -O https://www.sdcard.org/downloads/formatter_4/eula_mac/SDFormatter_4.00B.pkg` for mac, 

    `curl -O https://www.sdcard.org/downloads/formatter_4/eula_windows/SDFormatterv4.zip` for windows.

2. Follow the instructions to install the software.


3. Insert your SD card into the computer or laptop’s SD card reader and make a note of the drive letter allocated to it, e.g. `F:/`.

4. In SD Formatter, select the drive letter for your SD card and format it.

5. Download [NOOBS with raspbian pre-installed](https://downloads.raspberrypi.org/NOOBS_latest) or [NOOBS only](https://downloads.raspberrypi.org/NOOBS_lite_latest). Or:

    `curl -O https://downloads.raspberrypi.org/NOOBS_latest -L`, or
    `curl -O https://downloads.raspberrypi.org/NOOBS_lite_latest -L`

6. Extract the files from the zip.
7. Once your SD card has been formatted, drag all the files in the extracted NOOBS folder and drop them onto the SD card drive. The necessary files will then be transferred to your SD card.
8. When this process has finished, safely remove the SD card and insert it into your Raspberry Pi.

#### B. Use An Image 

You can choose the image on the [Download Page](https://www.raspberrypi.org/downloads/), Here we choose Raspbian.

Raspbian comes pre-installed with plenty of software for education, programming and general use. It has Python, Scratch, Sonic Pi, Java, Mathematica and more.

1. Download [RASPBIAN JESSIE WITH PIXEL](https://downloads.raspberrypi.org/raspbian_latest), Or [RASPBIAN JESSIE LITE](https://downloads.raspberrypi.org/raspbian_lite_latest)
2. Extract the files from the zip.
3. Visit [etcher.io](https://etcher.io/) and download and install the Etcher SD card image utility.

    [For OSX >= 10.9 ](https://resin-production-downloads.s3.amazonaws.com/etcher/1.0.0-beta.16/Etcher-1.0.0-beta.16-darwin-x64.dmg)
    [For Windows x64 installer](https://resin-production-downloads.s3.amazonaws.com/etcher/1.0.0-beta.16/Etcher-1.0.0-beta.16-win32-x64.exe)
4. Run Etcher and select the Raspbian image you unzipped on your computer or laptop.
5. Select the SD card drive. Note that the software may have already selected the right drive.
6. Finally, click Burn to transfer Raspbian to the SD card. You'll see a progress bar that tells you how much is left to do. Once complete, the utility will automatically eject/unmount the SD card so it's safe to remove it from the computer.

## SSH

### ENABLE SSH

The Raspberry Pi has an SSH server enabled by default. The SSH server on your Raspberry Pi may be disabled, in which case you will have to enable it manually. This is done using [`raspi-config`](https://www.raspberrypi.org/documentation/configuration/raspi-config.md):

Enter `sudo raspi-config` in the terminal, first select `advanced options`, then navigate to `ssh`, press `Enter` and select `Enable` or disable ssh server`.

### CONNECT

The default user name and password:

Username: __pi__

Password: __raspberry__

Use `arp -a` to find out the ip of Pi, then `ssh pi@PI_IP_ADDRESS` to connet to pi.

### VNC

#### A. ENABLING VNC SERVER GRAPHICALLY

On your Raspberry Pi, boot into the desktop.

Select __Menu > Preferences > Raspberry Pi Configuration > Interfaces__.

Ensure VNC is Enabled.

#### B. ENABLING VNC SERVER AT THE COMMAND LINE

You can enable VNC Server at the command line using `raspi-config`:

`sudo raspi-config`
Now, enable VNC Server by doing the following:

Navigate to `Advanced Options.`

Scroll down and select `VNC > Yes`.



