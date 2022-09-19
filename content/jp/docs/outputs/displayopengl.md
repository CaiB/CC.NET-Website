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
- **SmoothCircle:** Shows a ring of contiguous colour blocks. Optionally fades past colours away to infinity.
- **SmoothRadialFilled:** Highlights the currently active part of a static colour spectrum.
- **Tube:** (Experimental) Renders a moving tube which reacts to the beat and notes of the music.
- **Radar:** Shows a scanning circular display, with the newest colours being shown at the revolving marker.

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

### SmoothCircle ("Infinity Circle")
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothCircle.cs)

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| IsInfinity | bool | false |  | false just renders the ring. true also renders a decaying persistence effect, appearing to go off to infinity. |

### BlockMatrix
**Input Mode:** Discrete 2D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/BlockMatrix.cs)

2D version of BlockStrip. Resolution displayed adjusts to match the attached visualizer.  
No additional configuration is available.

### SmoothRadialFilled ("Circle Beamer")
**Input Modes:** Any  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/SmoothRadialFilled.cs)

Does not use the attached visualizer, but rather shows NoteFinder data directly.

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| BaseBrightness | float | 0.0 | 0.0~1.0 | How bright colours should be if there is no note at that location. Values greater than 0.0 show a ghost of the colour wheel at all times. |
| PeakWidth | float | 0.5 | 0.0~10.0 | How wide peaks should be. |
| BrightAmp | float | 1.0 | 0.0~100.0 | How much brightness should be amplified. If peak width is increased, you may want to increase this as well, and vice versa. |

### Radar
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/Radar.cs)

Spoke resolution (number of segments) is determined by the resolution of the attached visualizer.

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| Spokes | int | 100 | 1~10000 | How many spokes (history length / radial lines) there are. Higher shows more history. |
| Is3D | bool | false |  | Whether to show a height-variable, tilted-view version with beats causing vertical deflection of the surface. |
| FalloffAfter | float | 0.9 | 0.0~1.0 | How much of the radar is shown at full brightness, after which the rest has a gradient to black applied, ending in front of the "cursor". |

### NoiseField
**Input Mode:** Continuous 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/NoiseField.cs)

Fills the screen with procedural noise, shading with the current notes. Amount of the screen that is a colour is roughly proportional to how prominent that note is. Also attempts to do basic beat detection, and change the noise size in time. Very chaotic, flashing lights warning applies even more to this one!

| Option Name | Type | Default | Range | Description |
|---|---|---|---|---|
| Size1 | float | 8.0 | 0.0~10000.0 | Size scaling factor of the larger, beat-sensitive noise layer |
| Size2 | float | 3.5 | 0.0~10000.0 | Size scaling factor of the finer, constant noise layer |
| Speed | float | 5.0 | 0.0~10000.0 | How quickly the pattern appears to move up the screen |

### Tube
**Input Mode:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/Display/Tube.cs)

Circle resolution is determined by the resolution of the attached visualizer.  
You can move around with the W, A, S, D, Shift, Space keys. You can look around using the arrow keys. This is still extremely janky.  
No additional configuration is available.