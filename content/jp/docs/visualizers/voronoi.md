---
title: "Voronoi"
description: "Shows blobs of colour, size corresponding to relative note volume, and with inter-frame continuity."
lead: "Shows blobs of colour, size corresponding to relative note volume, and with inter-frame continuity."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 720
toc: true
---

**Output Mode:** Discrete 2D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/Voronoi.cs)

## Configuration

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Type | string |  | "Voronoi" | ❌ | Required: Specifies this visualizer type. |
| Name | string |  | Any unique name | ❌ | Required: A unique identifier used to attach outputs and controllers. |
| LEDCountX | int | 64 | 1~100000 | ✅ | The pixel count in the X dimension to output. |
| LEDCountY | int | 32 | 1~100000 | ✅ | The pixel count in the Y dimension to output. |
| FrameRate | int | 60 | 0~1000 | ✅ | The number of data frames to attempt to calculate per second. Determines how fast the data is output. |
| AmplifyPower | float | 2.51 | 0.0~100.0 | ✅ | The power to raise note amplitudes to in preprocessing. Larger values create a bigger difference in size between strong and weak notes. |
| DistancePower | float | 1.5 | 0.0~100.0 | ✅ | How far to draw colours from the blob center. Higher numbers means fewer individual colours will take up the majority of screen space. |
| Cutoff | float | 0.03 | 0.0~100.0 | ✅ | Notes below this threshold do not get considered when rendering. |
| CentersFromSides | bool | true |  | ✅ | Whether to draw the blobs around the screen in a predefined circular pattern (true), or to randomly place them on the screen (false). |
| SaturationAmplifier | float | 5.0 | 0.0~100.0 | ✅ | Amplifier for output colour saturation from note amplitude. |
| OutputGamma | float | 1.0 | 0.0~1.0 | ✅ | Scales the output saturation curve. |
| Enable | bool | true |  | ✅ | Whether to use this visualizer. |