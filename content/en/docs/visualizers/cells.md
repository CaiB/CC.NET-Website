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

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Type | string |  | "Cells" | ❌ | Required: Specifies this visualizer type. |
| Name | string |  | Any unique name | ❌ | Required: A unique identifier used to attach outputs and controllers. |
| LEDCount | int | 50 | 1~100000 | ✅ | The number of discrete data points to output. |
| FrameRate | int | 60 | 0~1000 | ✅ | The number of data frames to attempt to calculate per second. Determines how fast the data is output. |
| LightSiding | float | 1.9 | 0.0~100.0 | ✅ | How strongly inputs should be amplified before processing. Exponential. |
| SaturationAmplifier | float | 2.0 | 0.0~100.0 | ✅ | Multiplier for colour saturation before conversion to RGB and output. |
| QtyAmp | float | 20 | 0.0~100.0 | ✅ | Multiplier for LED quantity to turn on for the same input. Scale this with LED quantity. |
| SteadyBright | bool | false |  | ✅ | Smoothes LED brightness to reduce flickering. |
| TimeBased | bool | false |  | ✅ | Whether lights get added from the left side creating a time-dependent decay pattern, or are added randomly. |
| Snakey | bool | false |  | ❌ | Currently does nothing, like in cnlohr's ColorChord. |
| Enable | bool | true |  | ✅ | Whether to use this visualizer. |