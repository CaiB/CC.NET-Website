---
title: "Voronoi"
description: "Shows blobs of colour, size corresponding to relative note volume, and with inter-frame continuity."
lead: "Shows blobs of colour, size corresponding to relative note volume, and with inter-frame continuity."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 220
toc: true
---

**Output Mode:** Discrete 2D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/Voronoi.cs)

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
            <td>"Voronoi"</td>
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
            <td>LEDCountX</td>
            <td>int</td>
            <td>64</td>
            <td>1~100000</td>
            <td>The pixel count in the X dimension to output.</td>
        </tr>
        <tr>
            <td>LEDCountY</td>
            <td>int</td>
            <td>32</td>
            <td>1~100000</td>
            <td>The pixel count in the Y dimension to output.</td>
        </tr>
        <tr>
            <td>FrameRate</td>
            <td>int</td>
            <td>60</td>
            <td>0~1000</td>
            <td>The number of data frames to attempt to calculate per second. Determines how fast the data is output.</td>
        </tr>
        <tr>
            <td>AmplifyPower</td>
            <td>float</td>
            <td>2.51</td>
            <td>0.0~100.0</td>
            <td>The power to raise note amplitudes to in preprocessing. Larger values create a bigger difference in size between strong and weak notes.</td>
        </tr>
        <tr>
            <td>DistancePower</td>
            <td>float</td>
            <td>1.5</td>
            <td>0.0~100.0</td>
            <td>How far to draw colours from the blob center. Higher numbers means fewer individual colours will take up the majority of screen space.</td>
        </tr>
        <tr>
            <td>Cutoff</td>
            <td>float</td>
            <td>0.03</td>
            <td>0.0~100.0</td>
            <td>Notes below this threshold do not get considered when rendering.</td>
        </tr>
        <tr>
            <td>CentersFromSides</td>
            <td>bool</td>
            <td>true</td>
            <td></td>
            <td>Whether to draw the blobs around the screen in a predefined circular pattern (true), or to randomly place them on the screen (false).</td>
        </tr>
        <tr>
            <td>SaturationAmplifier</td>
            <td>float</td>
            <td>5.0</td>
            <td>0.0~100.0</td>
            <td>Amplifier for output colour saturation from note amplitude.</td>
        </tr>
        <tr>
            <td>OutputGamma</td>
            <td>float</td>
            <td>1.0</td>
            <td>0.0~1.0</td>
            <td>Scales the output saturation curve.</td>
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

