---
title: "NoteFinder"
description: "The module that takes audio data and converts it to note information."
lead: "The module that takes audio data and converts it to note information."
draft: false
images: []
menu: 
  docs:
    parent: "notefinder"
weight: 610
toc: true
---

There currently is only one NoteFinder, with the same behaviour as the one in base ColorChord.

[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/NoteFinder/BaseNoteFinder.cs)

## Configuration

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| StartFreq | float | 65.4064 | 0.0~20000.0 | ✅ | The minimum frequency analyzed. (in Hz) ℹ️ See note below. |
| DFTIIR | float | 0.65 | 0.0~1.0 | ✅ | Determines how much the previous frame's DFT data is used in the next frame. Smooths out rapid changes from frame-to-frame, but can cause delay if too strong. |
| DFTAmplify | float | 2.0 | 0.0~10000.0 | ✅ | Determines how much the raw DFT data is amplified before being used. |
| DFTSlope | float | 0.1 | 0.0~10000.0 | ✅ | The slope of the extra frequency-dependent amplification done to raw DFT data. Positive values increase sensitivity at higher frequencies. |
| OctaveFilterIterations | int | 2 | 0~10000 | ✅ | How often to run the octave data filter. This smoothes out each bin with adjacent ones. |
| OctaveFilterStrength | float | 0.5 | 0.0~1.0 | ✅ | How strong the octave data filter is. Higher values mean each bin is more aggressively averaged with adjacent bins. Higher values mean less glitchy, but also less clear note peaks. |
| NoteInfluenceDist | float | 1.8 | 0.0~100.0 | ✅ | How close a note needs to be to a distribution peak in order to be merged. |
| NoteAttachFreqIIR | float | 0.3 | 0.0~1.0 | ✅ | How strongly the note merging filter affects the note frequency. Stronger filter means notes take longer to shift positions to move together. |
| NoteAttachAmpIIR | float | 0.35 | 0.0~1.0 | ✅ | How strongly the note merging filter affects the note amplitude. Stronger filter means notes take longer to merge fully in amplitude. |
| NoteAttachAmpIIR2 | float | 0.25 | 0.0~1.0 | ✅ | This filter is applied to notes between cycles in order to smooth their amplitudes over time. |
| NoteCombineDistance | float | 0.5 | 0.0~100.0 | ✅ | How close two existing notes need to be in order to get combined into a single note. |
| NoteOutputChop | float | 0.05 | 0.0~1.0 | ✅ | Notes below this value get zeroed. Increase if low-amplitude notes are causing noise in output. |

{{< alert icon="ℹ️" text="The default configuration of StartFreq is different than cnlohr's implementation. If you want behaviour to match with his default configurations, change StartFreq to 55.0." />}}