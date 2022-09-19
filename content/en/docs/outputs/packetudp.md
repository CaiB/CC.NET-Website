---
title: "PacketUDP"
description: "Packs the data for each LED in sequence into a UDP packet, then sends it to a given IP/port."
lead: "Packs the data for each LED in sequence into a UDP packet, then sends it to a given IP/port."
draft: false
images: []
menu: 
  docs:
    parent: "outputs"
weight: 820
toc: true
---

**Input Modes:** Discrete 1D  
[Source Code](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/Outputs/PacketUDP.cs)


{{< alert icon="ℹ️" text="Individual packets have a size limit of 65,535 bytes. Each colour takes 1 byte." />}}


## Supported Protocols
- **Raw:** Outputs the LED data without any headers or packet splitting. Used by cnlohr's ColorChord and most basic receivers.
- **TPM2.net:** Outputs data packets as per the TPM2.net spec, [see here](https://gist.github.com/jblang/89e24e2655be6c463c56), and [TPM2 info in German](https://www.ledstyles.de/index.php?thread/18969-tpm2-protokoll-zur-matrix-lichtsteuerung/). Usable with [WLED](https://github.com/Aircoookie/WLED). Can split packets for many more LEDs.
- **E1.31:** Outputs the LED data in packets as per the [E1.31 spec](https://tsp.esta.org/tsp/documents/docs/ANSI_E1-31-2018.pdf). Useful for DMX systems.

## Common to all Protocols

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Type | string |  | "PacketUDP" | ❌ | Required: Specifies this output type. |
| Name | string |  | Any unique name | ❌ | Required: A unique identifier used to attach controllers. |
| VisualizerName | string |  | Valid visualizer instance name | ❌ | Required: The Name property of the visualizer instance to attach to. |
| Protocol | string | "Raw" | Supported protocols from list above | ❌ | Determines how packets are formatted and split. See below for supported protocols. cnlohr's version only supports "Raw". |
| IP | string | 127.0.0.1 | Valid IPs | ✅ | The IP to send the packets to. |
| Port | int | 7777 | 0~65535 | ✅ | The port to send the packets to. |
| LEDPattern | string | "RGB" | Any valid pattern | ❌ | The order in which to send data for each LED. Any combination of characters R, G, B, Y is valid, in any order, including repetition. Number of characters determines how many bytes each LED takes up in the packet. |
| ZigZag | bool | false |  | ❌ | Whether to reverse every odd line of the output for zig-zag wired LED arrays. |
| Mirror | bool | false |  | ❌ | Whether the LED matrix wiring runs left-to-right (false) or right-to-left (true). |
| RotatedArray | bool | false |  | ❌ | Rotates the LED data 180 degrees for an upside-down matrix. |
| SizeX | int | 1 |  | ❌ | How wide the LED matrix is. Leave as default if you're not using a matrix, otherwise specify correctly. |
| SizeY | int | 1 |  | ❌ | How tall the LED matrix is. Leave as default if you're not using a matrix, otherwise specify correctly. |
| StartIndex | int | 0 | 0~ | ✅ | Where in the visualizer data to start reading when putting data into the packet. Use this if you want the packet to only contain a subset of the visualizer data. |
| EndIndex | int | -1 | -1~ | ✅ | Where in the visualizer data to stop reading when putting data into the packet. Use this if you want the packet to only contain a subset of the visualizer data. -1 means read to the end, regardless of data amount. |
| Enable | bool | true |  | ✅ | Whether to use this output. |

## Configuring an LED Matrix
If you are outputting to an LED matrix, there are several options available to help you make sure the output is correct.  
{{< alert icon="❗" context="warning" text="If you use these options, you <b>must</b> specify SizeX and SizeY options correctly as well." />}}

Looking at your LED matrix from the front, match how it is wired with one of the images below to find the correct options for your setup:
<table class="table table-dark">
    <tbody>
        <tr> <!-- Linear patterns -->
            <td><img class="fit-image" src="Linear.png" /><br />ZigZag = false<br />Mirror = false<br />RotatedArray = false</td>
            <td><img class="fit-image" src="LinearMirror.png" /><br />ZigZag = false<br />Mirror = true<br />RotatedArray = false</td>
            <td><img class="fit-image img-rot-180" src="Linear.png" /><br />ZigZag = false<br />Mirror = false<br />RotatedArray = true</td>
            <td><img class="fit-image img-rot-180" src="LinearMirror.png" /><br />ZigZag = false<br />Mirror = true<br />RotatedArray = true</td>
        </tr>
        <tr> <!-- Serpentine patterns -->
            <td><img class="fit-image" src="Serpentine.png" /><br />ZigZag = true<br />Mirror = false<br />RotatedArray = false</td>
            <td><img class="fit-image" src="SerpentineMirror.png" /><br />ZigZag = true<br />Mirror = true<br />RotatedArray = false</td>
            <td><img class="fit-image img-rot-180" src="Serpentine.png" /><br />ZigZag = true<br />Mirror = false<br />RotatedArray = true</td>
            <td><img class="fit-image img-rot-180" src="SerpentineMirror.png" /><br />ZigZag = true<br />Mirror = true<br />RotatedArray = true</td>
        </tr>
    </tbody>
</table>

##  Protocol-Specific Configs
### Raw

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| PaddingFront | int | 0 | 0~1000 | ✅ | Number of padding bytes to append to the front of the packet. |
| PaddingBack | int | 0 | 0~1000 | ✅ | Number of padding bytes to append to the back of the packet. |
| PaddingContent | int | 0 | 0~255 | ✅ | What data to put in the padding bytes at the start and end, if present. |

### TPM2.net

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| PaddingContent | int | 0 | 0~255 | ✅ | What data to put in the padding bytes at the start and end, if present. |
| MaxPacketLength | int | -1 | -1~65535 | ❌ | The maximum size of packets before splitting. -1 means use protocol specified limit (1490). |
| ConstantPacketLength | bool | false |  | ❌ | Whether to make all packets the same size, filling blank space with PaddingContent. |

### E1.31 (sACN)

{{< alert icon="❗" context="warning" text="The maximum size of data in E1.31 is 512 bytes, which for RGB mode translates to 170 LEDs." />}}

| Option Name | Type | Default | Range | [Controllable](/docs/general/gettingstarted/#controllability) | Description |
|---|---|---|---|---|---|
| Universe | int | 1 | 1~63999 | ✅ | The DMX universe to address packets to. |
| UUID | hex string | 9E917B13714044CFB46F7A8298692DE3 | any 16-byte hex string | ❌ | Unique sender ID. Change this if you have multiple instances of ColorChord.NET on the network. |
