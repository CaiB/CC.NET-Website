---
title: "DisplayOpenGL"
description: "Uses OpenGL to render graphics to a window on the screen in various modes."
lead: "Uses OpenGL to render graphics to a window on the screen in various modes."
draft: false
images: []
menu: 
  docs:
    parent: "outputs"
weight: 300
toc: true
---

[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/DisplayOpenGL.cs)


## Modes Overview
- **BlockStrip:** Shows blocks of colour, similar to an LED strip, with the number of blocks controlled by the attached visualizer.
- **SmoothStrip:** Shows contiguous blocks of colour, with no resolution limit.
- **SmoothCircle:** Shows a ring of contiguous colour blocks. Optionally fades past colours away to infinity.
- **SmoothRadialFilled:** Highlights the currently active part of a static colour spectrum.
- **Tube:** (Experimental) Renders a moving tube which reacts to the beat and notes of the music.
- **Radar:** Shows a scanning circular display, with the newest colours being shown at the revolving marker.

## Window Configuration

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
            <td>"DisplayOpenGL"</td>
            <td><b>Required:</b> Specifies this output type.</td>
        </tr>
        <tr>
            <td>Name</td>
            <td>string</td>
            <td></td>
            <td>Any unique name</td>
            <td><b>Required:</b> A unique identifier used to attach controllers.</td>
        </tr>
        <tr>
            <td>VisualizerName</td>
            <td>string</td>
            <td></td>
            <td>Valid visualizer instance name</td>
            <td><b>Required:</b> The Name property of the visualizer instance to attach to.</td>
        </tr>
        <tr>
            <td>WindowWidth</td>
            <td>int</td>
            <td>1280</td>
            <td>10~4000</td>
            <td>The starting width of the window, in pixels.</td>
        </tr>
        <tr>
            <td>WindowHeight</td>
            <td>int</td>
            <td>720</td>
            <td>10~4000</td>
            <td>The starting height of the window, in pixels.</td>
        </tr>
        <tr>
            <td>Mode</td>
            <td>object array</td>
            <td>N/A</td>
            <td>Valid modes</td>
            <td>The mode(s) to use. See below for options and configurations. Currently only 1 supported at a time.</td>
        </tr>
    </tbody>
</table>

## Mode Information / Configurations

### BlockStrip
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/BlockStrip.cs)

Number of blocks displayed adjusts to match the attached visualizer.  
No additional configuration is available.

### SmoothStrip
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothStrip.cs)

No additional configuration is available.

### SmoothCircle ("Infinity Circle")
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothCircle.cs)

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
            <td>IsInfinity</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>false just renders the ring.<br />true also renders a decaying persistence effect, appearing to go off to infinity.</td>
        </tr>
    </tbody>
</table>

### BlockMatrix
**Input Mode:** Discrete 2D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/BlockMatrix.cs)

2D version of BlockStrip. Resolution displayed adjusts to match the attached visualizer.  
No additional configuration is available.

### SmoothRadialFilled ("Circle Beamer")
**Input Modes:** Any  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothRadialFilled.cs)

Does not use the attached visualizer, but rather shows NoteFinder data directly.

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
            <td>BaseBrightness</td>
            <td>float</td>
            <td>0.0</td>
            <td>0.0~1.0</td>
            <td>How bright colours should be if there is no note at that location. Values greater than 0.0 show a ghost of the colour wheel at all times.</td>
        </tr>
        <tr>
            <td>PeakWidth</td>
            <td>float</td>
            <td>0.5</td>
            <td>0.0~10.0</td>
            <td>How wide peaks should be.</td>
        </tr>
        <tr>
            <td>BrightAmp</td>
            <td>float</td>
            <td>1.0</td>
            <td>0.0~100.0</td>
            <td>How much brightness should be amplified. If peak width is increased, you may want to increase this as well, and vice versa.</td>
        </tr>
    </tbody>
</table>

### Radar
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/Radar.cs)

Spoke resolution (number of segments) is determined by the resolution of the attached visualizer.

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
            <td>Spokes</td>
            <td>int</td>
            <td>100</td>
            <td>1~10000</td>
            <td>How many spokes (history length / radial lines) there are. Higher shows more history.</td>
        </tr>
        <tr>
            <td>Is3D</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>Whether to show a height-variable, tilted-view version with beats causing vertical deflection of the surface.</td>
        </tr>
        <tr>
            <td>FalloffAfter</td>
            <td>float</td>
            <td>0.9</td>
            <td>0.0~1.0</td>
            <td>How much of the radar is shown at full brightness, after which the rest has a gradient to black applied, ending in front of the "cursor".</td>
        </tr>
    </tbody>
</table>

### NoiseField
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/NoiseField.cs)

Fills the screen with procedural noise, shading with the current notes. Amount of the screen that is a colour is roughly proportional to how prominent that note is. Also attempts to do basic beat detection, and change the noise size in time. Very chaotic, flashing lights warning applies even more to this one!

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
            <td>Size1</td>
            <td>float</td>
            <td>8.0</td>
            <td>0.0~10000.0</td>
            <td>Size scaling factor of the larger, beat-sensitive noise layer</td>
        </tr>
        <tr>
            <td>Size2</td>
            <td>float</td>
            <td>3.5</td>
            <td>0.0~10000.0</td>
            <td>Size scaling factor of the finer, constant noise layer</td>
        </tr>
        <tr>
            <td>Speed</td>
            <td>float</td>
            <td>5.0</td>
            <td>0.0~10000.0</td>
            <td>How quickly the pattern appears to move up the screen</td>
        </tr>
    </tbody>
</table>

### Tube
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/Tube.cs)

Circle resolution is determined by the resolution of the attached visualizer.  
You can move around with the W, A, S, D, Shift, Space keys. You can look around using the arrow keys. This is still extremely janky.  
No additional configuration is available.