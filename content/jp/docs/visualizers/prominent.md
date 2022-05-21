---
title: "Prominent"
description: "Shows a single colour of the most prominent note."
lead: "Shows a single colour of the most prominent note."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 740
toc: true
---

**Output Modes:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/Prominent.cs)

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
            <td>"Prominent"</td>
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
            <td>LEDCount</td>
            <td>int</td>
            <td>50</td>
            <td>1~100000</td>
            <td>The number of discrete data points to output. All LEDs will be set to the same colour.</td>
        </tr>
        <tr>
            <td>FrameRate</td>
            <td>int</td>
            <td>60</td>
            <td>0~1000</td>
            <td>The number of data frames to attempt to calculate per second. Determines how fast the data is output.</td>
        </tr>
        <tr>
            <td>SaturationAmplifier</td>
            <td>float</td>
            <td>2.0</td>
            <td>0.0~100.0</td>
            <td>Multiplier for colour saturation before conversion to RGB and output.</td>
        </tr>
        <tr>
            <td>Enable</td>
            <td>bool</td>
            <td>true</td>
            <td></td>
            <td>Whether to use this visualizer.</td>
        </tr>
    </tbody>
</table>
