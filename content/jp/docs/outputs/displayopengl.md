---
title: "DisplayOpenGL"
description: "Uses OpenGL to render graphics to a window on the screen in various modes."
lead: "Uses OpenGL to render graphics to a window on the screen in various modes."
draft: false
images: []
menu: 
  docs:
    parent: "outputs"
weight: 810
toc: true
---

[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/DisplayOpenGL.cs)


## Modes Overview
- **BlockStrip:** Shows blocks of colour, similar to an LED strip, with the number of blocks controlled by the attached visualizer.
- **SmoothStrip:** Shows contiguous blocks of colour, with no resolution limit.
- **BlockMatrix:** Shows blocks of colour on a 2D field, with resolution controlled by the attached visualizer.
- **SmoothCircle:** Shows a ring of contiguous colour blocks. Optionally fades past colours away to infinity.
- **Radar:** Shows a scanning circular display, with the newest colours being shown at the revolving marker.
- **SmoothRadialFilled:** Highlights the currently active part of a static colour spectrum.
- **RadialPoles:** Shows coloured poles sticking out from the center of the screen, with dynamic height.
- **ColorRibbon:** Shows a dynamic flying ribbon of colour trailing off into space, surrounded by a field of stars.
- **NoiseField:** Fills the screen with very chaotic procedural noise, shaded with note data.
- **CloudChamber:** Shows radial laser beams.
- **Tube:** (Experimental) Renders a moving tube which reacts to the beat and notes of the music.

## Window Configuration

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Type | string |  | "DisplayOpenGL" | ❌ | Required: Specifies this output type. |
| Name | string |  | Any unique name | ❌ | Required: A unique identifier used to attach controllers. |
| VisualizerName | string |  | Valid visualizer instance name | ❌ | Required: The Name property of the visualizer instance to attach to. |
| WindowWidth | int | 1280 | 10~4000 | ✅ | The starting width of the window, in pixels. |
| WindowHeight | int | 720 | 10~4000 | ✅ | The starting height of the window, in pixels. |
| Enable | bool | true | | ✅ | Whether to show the window and process visualizer data. |
| Mode | object array | N/A | Valid modes | ❌ | The mode(s) to use. See below for options and configurations. Currently only 1 supported at a time. |

## Mode Information / Configurations

Display modes are not controllable yet.

### BlockStrip
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/BlockStrip.cs)

Number of blocks displayed adjusts to match the attached visualizer.  
No additional configuration is available.

### SmoothStrip
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothStrip.cs)

No additional configuration is available.

### BlockMatrix
**Input Mode:** Discrete 2D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/BlockMatrix.cs)

2D version of BlockStrip. Resolution displayed adjusts to match the attached visualizer.  
No additional configuration is available.

### SmoothCircle ("Infinity Circle")
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothCircle.cs)

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| IsInfinity | bool | false |  | false just renders the ring. true also renders a decaying persistence effect, appearing to go off to infinity. |

### Radar
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/Radar.cs)

Spoke resolution (number of segments) is determined by the resolution of the attached visualizer.

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| Spokes | int | 100 | 1~10,000 | How many spokes (history length / radial lines) there are. Higher shows more history. |
| Is3D | bool | false |  | Whether to show a height-variable, tilted-view version with beats causing vertical deflection of the surface. |
| FalloffAfter | float | 0.9 | 0.0~1.0 | How much of the radar is shown at full brightness, after which the rest has a gradient to black applied, ending in front of the "cursor". |

### SmoothRadialFilled ("Circle Beamer")
**Input Modes:** Any  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothRadialFilled.cs)

Does not use the attached visualizer, but rather shows NoteFinder data directly.

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| BaseBrightness | float | 0.0 | 0.0~1.0 | How bright colours should be if there is no note at that location. Values greater than 0.0 show a ghost of the colour wheel at all times. |
| PeakWidth | float | 0.5 | 0.0~10.0 | How wide peaks should be. |
| BrightAmp | float | 1.0 | 0.0~100.0 | How much brightness should be amplified. If peak width is increased, you may want to increase this as well, and vice versa. |

### RadialPoles
**Input Modes:** Any  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/RadialPoles.cs)

Does not use the attached visualizer, but rather shows NoteFinder data directly.

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| CenterSize | float | 0.2 | 0.0~1.0 | How much of the center is left blank before the poles begin. 0 leaves no gap at the center. |
| ScaleExponent | float | 1.6 | 0.0~10.0 | The exponent to scale note data by when calculating pole height. |
| ScaleFactor | float | 0.7 | 0.0~100.0 | The multiplier to scale note data by when calculating pole height. |

### ColorRibbon
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/ColorRibbon.cs)

Displays a flying ribbon of colour trailing off into space, surrounded by a field of stars that also display colour. The ribbon grows and shrinks with note strength.  
BETA: The star rendering is still quite janky and needs more work to look good.

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| RibbonLength | int | 120 | 2~10,000 | How many frames of history the ribbon shows along its length. |
| RibbonScale | float | 0.3 | 0.0~100.0 | The base width of the ribbon, which will be scaled by the music. |
| StarCount | int | 1000 | 0~100,000 | How many stars are shown in the background. |
| StarSpeed | float | 0.1 | 0.0~100.0 | How fast the stars in the background move away. |
| StarSize | float | 0.05 | 0.0~1.0 | How large the stars appear. |

### NoiseField
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/NoiseField.cs)

Fills the screen with procedural noise, shading with the current notes. Amount of the screen that is a colour is roughly proportional to how prominent that note is. Also attempts to do basic beat detection, and change the noise size in time. Very chaotic, flashing lights warning applies even more to this one!

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| Size1 | float | 8.0 | 0.0~10,000.0 | Size scaling factor of the larger, beat-sensitive noise layer |
| Size2 | float | 3.5 | 0.0~10,000.0 | Size scaling factor of the finer, constant noise layer |
| Speed | float | 5.0 | 0.0~10,000.0 | How quickly the pattern appears to move up the screen |

### CloudChamber
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/CloudChamber.cs)

Currently displays radial laser beams, unfinished.

No additional configuration is available.


### Tube
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/Tube.cs)

Circle resolution is determined by the resolution of the attached visualizer.  
You can move around with the W, A, S, D, Shift, Space keys. You can look around using the arrow keys. This is still extremely janky.  
No additional configuration is available.