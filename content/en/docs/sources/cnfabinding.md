---
title: "CNFABinding"
description: "Uses cnlohr's CNFA for compatibility with non-Windows platforms."
lead: "Uses cnlohr's CNFA for compatibility with non-Windows platforms."
draft: false
images: []
menu: 
  docs:
    parent: "sources"
weight: 110
toc: true
---

[CNFA](https://github.com/cntools/cnfa) provides compatibility with a variety of different audio systems on various platforms. It is currently the only supported method of receiving audio on Linux. On Windows, [WASAPILoopback](../wasapiloopback) is recommended instead.

[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Sources/CNFABinding.cs)

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
            <td>Driver</td>
            <td>string</td>
            <td>"AUTO"</td>
            <td>"AUTO", "ALSA", "ANDROID", "NULL", "PULSE", "WASAPI", "WIN"</td>
            <td>Determines which CNFA driver module will be used. If "AUTO" is specified, it will attempt to find the best driver for your system.</td>
        </tr>
        <tr>
            <td>SampleRate</td>
            <td>int</td>
            <td>48000</td>
            <td>8000~384000</td>
            <td>Suggests a sample rate to the driver.</td>
        </tr>
        <tr>
            <td>ChannelCount</td>
            <td>int</td>
            <td>2</td>
            <td>1~20</td>
            <td>Suggests a channel count to the driver.</td>
        </tr>
        <tr>
            <td>BufferSize</td>
            <td>int</td>
            <td>480</td>
            <td>1~10000</td>
            <td>Suggests a buffer size to the driver.</td>
        </tr>
        <tr>
            <td>Device</td>
            <td>string</td>
            <td>"default"</td>
            <td>Valid device names / keywords</td>
            <td>The recording device to use. This depends on the driver. Please check CNFA documentation for the driver you want to use to determine what should be used here.</td>
        </tr>
        <tr>
            <td>DeviceOutput</td>
            <td>string</td>
            <td>"default"</td>
            <td>Valid device names / keywords</td>
            <td>The output device to use. This device is not actually used, as ColorChord.NET does not play audio.</td>
        </tr>
    </tbody>
</table>