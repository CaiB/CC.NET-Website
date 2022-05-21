---
title: "Use with OpenRGB"
description: "ColorChord.NET can be used with OpenRGB to control PC components and peripherals with RGB LEDs."
lead: "ColorChord.NET can be used with OpenRGB to control PC components and peripherals with RGB LEDs."
draft: false
images: []
menu: 
  docs:
    parent: "general"
weight: 170
toc: true
---

Currently the easiest way to pipe data from ColorChord.NET into OpenRGB is via E1.31 packets. These instructions will help you set up everything you need.

I have only tested this on Windows, but it theoretically should work on Linux as well.

## Install & Configure OpenRGB
<a class="btn btn-primary btn-lg px-4 mb-2" href="https://gitlab.com/CalcProgrammer1/OpenRGB/-/releases" role="button">Download OpenRGB</a>
<a class="btn btn-primary btn-lg px-4 mb-2" href="https://gitlab.com/OpenRGBDevelopers/OpenRGBE131ReceiverPlugin/-/pipelines" role="button">Download E1.31 Plugin</a>
1) Start by downloading OpenRGB and the E1.31 Receiver Plugin for your system from the above links.  
    a) For the plugin, click the newest passing build's pipeline number, select your platform, then click "Download Job Artifacts" on the right.
2) For Windows, unzip the main package to a convenient location, such as `C:\Program Files\OpenRGB`. For Linux, follow the [install instructions here](https://gitlab.com/CalcProgrammer1/OpenRGB#linux).
3) On Windows, run OpenRGB as administrator (this is only needed the first time). Once it has opened, close it.
4) Install the E1.31 receiver plugin:  
    a) For Windows, copy the DLL file from the archive you downloaded to `%APPDATA%/OpenRGB/plugins`.  
    b) For Linux, copy the SO file from the archive you downloaded to `~/.config/OpenRGB/plugins`.
5) Start OpenRGB. You should see a "E1.31 Receiver" tab in the GUI.
6) On the "Devices" tab, select the devices you'd like to configure. For each, change the mode to "Direct".
7) On the "E1.31 Receiver" tab, click "Add Universe". Click the newly added universe in the right pane, then click the device you'd like to control from the left pane. With both selected, click "Add Controller". To be able to read the universe number, you may have to enlarge the window.
8) Adjust the Start Channel, Start LED, and End LED if necessary, and repeat for all of your devices. If you want a group of devices to follow the same lighting pattern, put them in the same universe.
9) Click "Start Receiver" and allow OpenRGB through the firewall if necessary.

## Install & Configure ColorChord.NET
<a class="btn btn-primary btn-lg px-4 mb-2" href="https://www.colorchord.net/docs/general/gettingstarted/" role="button">Set Up ColorChord.NET</a>
1) Follow the instructions in the Getting Started guide to install ColorChord.NET.
2) Add or edit an output to be of type `PacketUDP`. Apply the following settings:
```json
"Type": "PacketUDP",
"Name": "CC.NET to OpenRGB",
"VisualizerName": "NameHere",
"IP": "127.0.0.1",
"Port": 5568,
"Protocol": "E1.31",
"Universe": 1,
"Enable": true
```
3) Make sure to replace the `VisualizerName` entry with your visualier instance name, and replace `Universe` with the universe number that OpenRGB is expecting.
4) Start ColorChord.NET, verify there are no config issues in the console, and check to make sure the "Packets Received" number is increasing in OpenRGB. Your device should now be lighting up with ColorChord.NET!
5) As this is a network output, the computer on which ColorChord.NET is running does not have to be the same computer as OpenRGB is running. You can also control OpenRGB across multiple computers, by simply adding more `PacketUDP` entries in the ColorChord.NET config, and changing the options as appropriate for each one. Musticast should also work to simultaneously send to multiple receivers.