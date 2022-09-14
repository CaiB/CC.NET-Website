---
title: "Linear"
description: "Shows contiguous blocks of colour, size corresponding to relative note volume, and with inter-frame continuity."
lead: "Shows contiguous blocks of colour, size corresponding to relative note volume, and with inter-frame continuity."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 710
toc: true
---

**Output Modes:** Discrete 1D, Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/Linear.cs)

## Configuration

<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">Option Name</th>
            <th scope="col">Type</th>
            <th scope="col">Default</th>
            <th scope="col">Range</th>
            <th scope="col"><a href="/docs/general/gettingstarted/#controllability">Controllable</a></th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Type</td>
            <td>string</td>
            <td></td>
            <td>"Linear"</td>
            <td>❌</td>
            <td><b>Required:</b> Specifies this visualizer type.</td>
        </tr>
        <tr>
            <td>Name</td>
            <td>string</td>
            <td></td>
            <td>Any unique name</td>
            <td>❌</td>
            <td><b>Required:</b> A unique identifier used to attach outputs and controllers.</td>
        </tr>
        <tr>
            <td>LEDCount</td>
            <td>int</td>
            <td>50</td>
            <td>1~100000</td>
            <td>✅</td>
            <td>The number of discrete data points to output. Set this to a low value like 24 if only continuous output is used to save CPU time.</td>
        </tr>
        <tr>
            <td>FrameRate</td>
            <td>int</td>
            <td>60</td>
            <td>0~1000</td>
            <td>✅</td>
            <td>The number of data frames to attempt to calculate per second. Determines how fast the data is output.</td>
        </tr>
        <tr>
            <td>LEDFloor</td>
            <td>float</td>
            <td>0.1</td>
            <td>0.0~1.0</td>
            <td>✅</td>
            <td>The minimum intensity of an LED, before it is output as off instead.</td>
        </tr>
        <tr>
            <td>LightSiding</td>
            <td>float</td>
            <td>1.0</td>
            <td>0.0~100.0</td>
            <td>✅</td>
            <td>How strongly inputs should be amplified before processing. Exponential.</td>
        </tr>
        <tr>
            <td>SaturationAmplifier</td>
            <td>float</td>
            <td>2.0</td>
            <td>0.0~100.0</td>
            <td>✅</td>
            <td>Multiplier for colour saturation before conversion to RGB and output.</td>
        </tr>
        <tr>
            <td>IsCircular</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>✅</td>
            <td>Whether to treat the output as a circle, allowing wrap-around, or as a line with defined ends.<br />ℹ️ See below note.</td>
        </tr>
        <tr>
            <td>SteadyBright</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>✅</td>
            <td>Smoothes LED brightness to reduce flickering.</td>
        </tr>
        <tr>
            <td>LEDLimit</td>
            <td>float</td>
            <td>1.0</td>
            <td>0.0~1.0</td>
            <td>✅</td>
            <td>The maximum LED brightness. Caps all LEDs at this value, but does not affect values below this threshold.</td>
        </tr>
        <tr>
            <td>Enable</td>
            <td>bool</td>
            <td>true</td>
            <td></td>
            <td>✅</td>
            <td>Whether to use this visualizer.</td>
        </tr>
    </tbody>
</table>

{{< alert icon="ℹ️" text="IsCircular=true in continuous mode does not match the behaviour of base ColorChord, as it uses a different, custom algorithm for positioning. However, discrete mode should match the base version.<br />IsCircular=false should match base ColorChord in both continuous and discrete mode." />}}