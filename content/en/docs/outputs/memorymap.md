---
title: "MemoryMap"
description: "Creates a non-persistent, memory-mapped file (only in RAM, not on disk), then writes the LED count and data into the file at every frame."
lead: "Creates a non-persistent, memory-mapped file (only in RAM, not on disk), then writes the LED count and data into the file at every frame."
draft: false
images: []
menu: 
  docs:
    parent: "outputs"
weight: 320
toc: true
---

**Input Modes:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/MemoryMap.cs)  
This output is useful if you want unrelated processes to be able to read the data. Acts as a Windows equivalent to the SHM output of cnlohr's ColorChord.  

The name of the memory-mapped file will be "ColorChord.NET-[Name]", and the created mutex will be named "ColorChord.NET-Mutex-[Name]", where [Name] is the instance name from the configuration.

Notes:
- Number of reading processes is not limited
- Timing/frame rate synchronization is not provided
- Reading processes should also lock the provided Mutex during data reads

Data format is:

    [uint32 LEDCount]  {[uint8 R] [uint8 G] [uint8 B]} x LEDCount

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
            <td>Type</td>
            <td>string</td>
            <td></td>
            <td>"MemoryMap"</td>
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
    </tbody>
</table>
