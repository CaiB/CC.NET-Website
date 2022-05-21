---
title: "UDPReceiver1D (For Debugging)"
description: "Takes in UDP packets, and outputs them as if the data were locally calculated. Does not actually use the sources or NoteFinder in this instance. Rate and size is determined by input packets."
lead: "Takes in UDP packets, and outputs them as if the data were locally calculated. Does not actually use the sources or NoteFinder in this instance. Rate and size is determined by input packets."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 760
toc: true
---

**Output Modes:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/UDPReceiver.cs)

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
            <td>Type</td>
            <td>string</td>
            <td></td>
            <td>"UDPReceiver1D"</td>
            <td><b>Required:</b> Specifies this visualizer type.</td>
        </tr>
        <tr>
            <td>Name</td>
            <td>string</td>
            <td></td>
            <td>Any unique name</td>
            <td><b>Required:</b> A unique identifier used to attach outputs and controllers.</td>
        </tr>
        <tr>
            <td>HasYellowChannel</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>Whether to interpret packets as RGB (false) or RGBY (true).</td>
        </tr>
        <tr>
            <td>Port</td>
            <td>int</td>
            <td>7777</td>
            <td>0~65535</td>
            <td>The port to listen on for UDP packets.</td>
        </tr>
    </tbody>
</table>

{{< alert icon="ℹ️" text="Make sure your firewall settings allow ColorChord.NET to receive UDP packets." />}}