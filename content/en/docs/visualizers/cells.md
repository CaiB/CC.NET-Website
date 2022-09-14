---
title: "Cells"
description: "Shows individual LEDs appearing and decaying, in a scattered pattern depending on surroundings and time."
lead: "Shows individual LEDs appearing and decaying, in a scattered pattern depending on surroundings and time."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 720
toc: true
---

**Output Modes:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/Cells.cs)

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
            <td>"Cells"</td>
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
            <td>The number of discrete data points to output.</td>
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
            <td>LightSiding</td>
            <td>float</td>
            <td>1.9</td>
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
            <td>QtyAmp</td>
            <td>float</td>
            <td>20</td>
            <td>0.0~100.0</td>
            <td>✅</td>
            <td>Multiplier for LED quantity to turn on for the same input. Scale this with LED quantity.</td>
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
            <td>TimeBased</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>✅</td>
            <td>Whether lights get added from the left side creating a time-dependent decay pattern, or are added randomly.</td>
        </tr>
        <tr>
            <td>Snakey</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>❌</td>
            <td>Currently does nothing, like cnlohr's ColorChord.</td>
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