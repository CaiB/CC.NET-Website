---
title: "WASAPILoopback"
description: "Modern Windows audio interface, supports both inputs and loopback."
lead: "Modern Windows audio interface, supports both inputs and loopback."
draft: false
images: []
menu: 
  docs:
    parent: "sources"
weight: 100
toc: true
---

The Windows Audio Session API is the recommended way of sourcing low-latency audio data on Windows 7 and later systems. It supports both input devices (such as microphones), and output devices (loopback from speaker output).

[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Sources/WASAPILoopback.cs). Uses [Vannatech's netcoreaudio](https://github.com/dorba/netcoreaudio).

## Configuration
<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">Option Name</th>
            <th scope="col">Type</th>
            <th scope="col">Default</th>
            <th scope="col">Range</th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Device</td>
            <td>string</td>
            <td>"default"</td>
            <td>"default",<br />"defaultInput",<br /><s>"defaultTracking"</s>,<br />Device IDs</td>
            <td>If "default", then the default render (output) device at the time of startup will be used.<br />If "defaultInput", then the default capture (input) device at the time of startup will be used.<br /><s>If "defaultTracking", the default device will be used, and will keep up with changes to the default, switching as the system does</s> (not yet implemented).<br />If a device ID is specified, that device is used, but if it is not found, then behaviour reverts to "default".</td>
        </tr>
        <tr>
            <td>ShowDeviceInfo</td>
            <td>bool</td>
            <td>true</td>
            <td></td>
            <td>If true, outputs currently connected devices and their IDs at startup, to help you find a device.</td>
        </tr>
    </tbody>
</table>

## Device IDs
Device IDs are unique for each device on the system, vary between different computers, and only change if drivers are updated/changed. Removal and re-attachment of a USB device will not change the ID. They are not readily visible to the user, but other software using WASAPI will have access to the same IDs. Use ShowDeviceInfo (above) to find the ID for your preferred device. Output format is:

    [Index] "Device Name" = "Device ID"  
(Index is not used, it is just present to make the list easier to read)