---
title: "NoteFinder"
description: "The module that takes audio data and converts it to note information."
lead: "The module that takes audio data and converts it to note information."
draft: false
images: []
menu: 
  docs:
    parent: "notefinder"
weight: 150
toc: true
---

There currently is only one NoteFinder, with the same behaviour as the one in base ColorChord.

[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/NoteFinder/BaseNoteFinder.cs)

## Configuration

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
            <td>StartFreq</td>
            <td>float</td>
            <td>65.4064</td>
            <td>0.0~20000.0</td>
            <td>The minimum frequency analyzed. (in Hz)<br />ℹ️ See note below.</td>
        </tr>
        <tr>
            <td>DFTIIR</td>
            <td>float</td>
            <td>0.65</td>
            <td>0.0~1.0</td>
            <td>Determines how much the previous frame's DFT data is used in the next frame. Smooths out rapid changes from frame-to-frame, but can cause delay if too strong.</td>
        </tr>
        <tr>
            <td>DFTAmplify</td>
            <td>float</td>
            <td>2.0</td>
            <td>0.0~10000.0</td>
            <td>Determines how much the raw DFT data is amplified before being used.</td>
        </tr>
        <tr>
            <td>DFTSlope</td>
            <td>float</td>
            <td>0.1</td>
            <td>0.0~10000.0</td>
            <td>The slope of the extra frequency-dependent amplification done to raw DFT data. Positive values increase sensitivity at higher frequencies.</td>
        </tr>
        <tr>
            <td>OctaveFilterIterations</td>
            <td>int</td>
            <td>2</td>
            <td>0~10000</td>
            <td>How often to run the octave data filter. This smoothes out each bin with adjacent ones.</td>
        </tr>
        <tr>
            <td>OctaveFilterStrength</td>
            <td>float</td>
            <td>0.5</td>
            <td>0.0~1.0</td>
            <td>How strong the octave data filter is. Higher values mean each bin is more aggressively averaged with adjacent bins. Higher values mean less glitchy, but also less clear note peaks.</td>
        </tr>
        <tr>
            <td>NoteInfluenceDist</td>
            <td>float</td>
            <td>1.8</td>
            <td>0.0~100.0</td>
            <td>How close a note needs to be to a distribution peak in order to be merged.</td>
        </tr>
        <tr>
            <td>NoteAttachFreqIIR</td>
            <td>float</td>
            <td>0.3</td>
            <td>0.0~1.0</td>
            <td>How strongly the note merging filter affects the note frequency. Stronger filter means notes take longer to shift positions to move together.</td>
        </tr>
        <tr>
            <td>NoteAttachAmpIIR</td>
            <td>float</td>
            <td>0.35</td>
            <td>0.0~1.0</td>
            <td>How strongly the note merging filter affects the note amplitude. Stronger filter means notes take longer to merge fully in amplitude.</td>
        </tr>
        <tr>
            <td>NoteAttachAmpIIR2</td>
            <td>float</td>
            <td>0.25</td>
            <td>0.0~1.0</td>
            <td>This filter is applied to notes between cycles in order to smooth their amplitudes over time.</td>
        </tr>
        <tr>
            <td>NoteCombineDistance</td>
            <td>float</td>
            <td>0.5</td>
            <td>0.0~100.0</td>
            <td>How close two existing notes need to be in order to get combined into a single note.</td>
        </tr>
        <tr>
            <td>NoteOutputChop</td>
            <td>float</td>
            <td>0.05</td>
            <td>0.0~1.0</td>
            <td>Notes below this value get zeroed. Increase if low-amplitude notes are causing noise in output.</td>
        </tr>
    </tbody>
</table>

{{< alert icon="ℹ️" text="The default configuration of StartFreq is different than cnlohr's implementation. If you want behaviour to match with his default configurations, change StartFreq to 55.0." />}}