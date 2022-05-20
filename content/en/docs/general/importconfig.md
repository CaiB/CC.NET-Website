---
title: "Import Config from ColorChord"
description: "A cnlohr ColorChord configuration file can be converted to a ColorChord.NET configuration with a bit of manual work."
lead: "A cnlohr ColorChord configuration file can be converted to a ColorChord.NET configuration with a bit of manual work."
draft: false
images: []
menu: 
  docs:
    parent: "general"
weight: 60
toc: true
---

There currently is no automatic way to convert a config file, due to the difference in functionality.

The parameters for almost everything in ColorChord.NET are backwards-compatible with cnlohr's ColorChord, so you can usually copy the **values**. If there is a difference, it should be noted in the relevant section for the component.

If you want to replicate an existing setup, start with the default config.json file, and copy parameters over to my format one-by-one, finding the appropriate place to put them.

I try to maintain the same behaviour given the same inputs as cnlohr's version. If you notice an undocumented difference, please let me know.

## Name Conversions

<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">cnlohr ColorChord Name</th>
            <th scope="col">ColorChord.NET Component</th>
            <th scope="col">ColorChord.NET Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>base_hz</td>
            <td>NoteFinder</td>
            <td>StartFreq</td>
        </tr>
        <tr>
            <td>dft_iir</td>
            <td>NoteFinder</td>
            <td>DFTIIR</td>
        </tr>
        <tr>
            <td>amplify</td>
            <td>NoteFinder</td>
            <td>DFTAmplify</td>
        </tr>
        <tr>
            <td>slope</td>
            <td>NoteFinder</td>
            <td>DFTSlope</td>
        </tr>
        <tr>
            <td>filter_iter</td>
            <td>NoteFinder</td>
            <td>OctaveFilterIterations</td>
        </tr>
        <tr>
            <td>filter_strength</td>
            <td>NoteFinder</td>
            <td>OctaveFilterStrength</td>
        </tr>
        <tr>
            <td>note_jumpability</td>
            <td>NoteFinder</td>
            <td>NoteInfluenceDist</td>
        </tr>
        <tr>
            <td>note_attach_freq_iir</td>
            <td>NoteFinder</td>
            <td>NoteAttachFreqIIR</td>
        </tr>
        <tr>
            <td>note_attach_amp_iir</td>
            <td>NoteFinder</td>
            <td>NoteAttachAmpIIR</td>
        </tr>
        <tr>
            <td>note_attach_amp_iir2</td>
            <td>NoteFinder</td>
            <td>NoteAttachAmpIIR2</td>
        </tr>
        <tr>
            <td>note_combine_distance</td>
            <td>NoteFinder</td>
            <td>NoteCombineDistance</td>
        </tr>
        <tr>
            <td>note_out_chop</td>
            <td>NoteFinder</td>
            <td>NoteOutputChop</td>
        </tr>
        <tr>
            <td>leds</td>
            <td>Visualizer: Cells, Linear</td>
            <td>LEDCount</td>
        </tr>
        <tr>
            <td>led_floor</td>
            <td>Visualizer: Linear</td>
            <td>LEDFloor</td>
        </tr>
        <tr>
            <td>light_siding</td>
            <td>Visualizer: Cells, Linear</td>
            <td>LightSiding</td>
        </tr>
        <tr>
            <td>satamp</td>
            <td>Visualizer: Cells, Linear</td>
            <td>SaturationAmplifier</td>
        </tr>
        <tr>
            <td>qtyamp</td>
            <td>Visualizer: Cells</td>
            <td>QtyAmp</td>
        </tr>
        <tr>
            <td>seady_bright<br/>steady_bright</td>
            <td>Visualizer: Cells, Linear</td>
            <td>SteadyBright</td>
        </tr>
        <tr>
            <td>timebased</td>
            <td>Visualizer: Cells</td>
            <td>TimeBased</td>
        </tr>
        <tr>
            <td>snakey</td>
            <td>Visualizer: Cells</td>
            <td>Snakey</td>
        </tr>
        <tr>
            <td>is_loop</td>
            <td>Visualizer: Linear</td>
            <td>IsCircular</td>
        </tr>
        <tr>
            <td>led_limit</td>
            <td>Visualizer: Linear</td>
            <td>LEDLimit</td>
        </tr>
        <tr>
            <td>skipfirst</td>
            <td>Output: PacketUDP/Raw</td>
            <td>PaddingFront</td>
        </tr>
        <tr>
            <td>firstval</td>
            <td>Output: PacketUDP/Raw</td>
            <td>PaddingContent</td>
        </tr>
    </tbody>
</table>