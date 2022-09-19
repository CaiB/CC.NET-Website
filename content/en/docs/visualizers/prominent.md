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

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Type | string |  | "Prominent" | ❌ | Required: Specifies this visualizer type. |
| Name | string |  | Any unique name | ❌ | Required: A unique identifier used to attach outputs and controllers. |
| LEDCount | int | 50 | 1~100000 | ✅ | The number of discrete data points to output. All LEDs will be set to the same colour. |
| FrameRate | int | 60 | 0~1000 | ✅ | The number of data frames to attempt to calculate per second. Determines how fast the data is output. |
| SaturationAmplifier | float | 2.0 | 0.0~100.0 | ✅ | Multiplier for colour saturation before conversion to RGB and output. |
| Enable | bool | true |  | ✅ | Whether to use this visualizer. |
