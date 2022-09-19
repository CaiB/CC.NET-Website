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

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Type | string |  | "Linear" | ❌ | Required: Specifies this visualizer type. |
| Name | string |  | Any unique name | ❌ | Required: A unique identifier used to attach outputs and controllers. |
| LEDCount | int | 50 | 1~100000 | ✅ | The number of discrete data points to output. Set this to a low value like 24 if only continuous output is used to save CPU time. |
| FrameRate | int | 60 | 0~1000 | ✅ | The number of data frames to attempt to calculate per second. Determines how fast the data is output. |
| LEDFloor | float | 0.1 | 0.0~1.0 | ✅ | The minimum intensity of an LED, before it is output as off instead. |
| LightSiding | float | 1.0 | 0.0~100.0 | ✅ | How strongly inputs should be amplified before processing. Exponential. |
| SaturationAmplifier | float | 2.0 | 0.0~100.0 | ✅ | Multiplier for colour saturation before conversion to RGB and output. |
| IsCircular | bool | false |  | ✅ | Whether to treat the output as a circle, allowing wrap-around, or as a line with defined ends. ℹ️ See below note. |
| SteadyBright | bool | false |  | ✅ | Smoothes LED brightness to reduce flickering. |
| LEDLimit | float | 1.0 | 0.0~1.0 | ✅ | The maximum LED brightness. Caps all LEDs at this value, but does not affect values below this threshold. |
| Enable | bool | true |  | ✅ | Whether to use this visualizer. |

{{< alert icon="ℹ️" text="IsCircular=true in continuous mode does not match the behaviour of base ColorChord, as it uses a different, custom algorithm for positioning. However, discrete mode should match the base version.<br />IsCircular=false should match base ColorChord in both continuous and discrete mode." />}}