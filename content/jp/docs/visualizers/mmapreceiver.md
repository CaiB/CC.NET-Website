---
title: "MemoryMapReceiver (For Debugging)"
description: "Reads from an existing memory-mapped file using an existing mutex. Intended for testing only, or as a reference implementation."
lead: "Reads from an existing memory-mapped file using an existing mutex. Intended for testing only, or as a reference implementations."
draft: false
images: []
menu: 
  docs:
    parent: "visualizers"
weight: 750
toc: true
---

**Output Modes:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Visualizers/MemoryMapReceiver.cs)

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
            <td>"MemoryMapReceiver"</td>
            <td><b>Required:</b> Specifies this visualizer type.</td>
        </tr>
        <tr>
            <td>Name</td>
            <td>string</td>
            <td></td>
            <td>Any unique name</td>
            <td><b>Required:</b> A unique identifier used to attach outputs and controllers.</td>
        </tr>
        <tr>
            <td>MapName</td>
            <td>string</td>
            <td>N/A</td>
            <td>Valid memory map name</td>
            <td>The name of the memory-mapped file to read data from. The MemoryMap output will create a file by name ColorChord.NET-[OutputName] where [OutputName] is the Name configuration parameter on the Output instance.</td>
        </tr>
        <tr>
            <td>MutexName</td>
            <td>string</td>
            <td>N/A</td>
            <td>Valid mutex name</td>
            <td>The name of the mutex to interface with. The MemoryMap output will create a mutex by name ColorChord.NET-Mutex-[OutputName] where [OutputName] is the Name configuration parameter on the Output instance.</td>
        </tr>
        <tr>
            <td>FrameRate</td>
            <td>int</td>
            <td>60</td>
            <td>0~1000</td>
            <td>The number of data frames to attempt to read per second. Determines how fast the data is output.</td>
        </tr>
    </tbody>
</table>