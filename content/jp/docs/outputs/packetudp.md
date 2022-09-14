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

<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">Option Name</th>
            <th scope="col">Type</th>
            <th scope="col">Default</th>
            <th scope="col">Range</th>
            <th scope="col"><a href="/docs/general/gettingstarted/#controllability">Controllable</a></th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Type</td>
            <td>string</td>
            <td></td>
            <td>"PacketUDP"</td>
            <td>❌</td>
            <td><b>Required:</b> Specifies this output type.</td>
        </tr>
        <tr>
            <td>Name</td>
            <td>string</td>
            <td></td>
            <td>Any unique name</td>
            <td>❌</td>
            <td><b>Required:</b> A unique identifier used to attach controllers.</td>
        </tr>
        <tr>
            <td>VisualizerName</td>
            <td>string</td>
            <td></td>
            <td>Valid visualizer instance name</td>
            <td>❌</td>
            <td><b>Required:</b> The Name property of the visualizer instance to attach to.</td>
        </tr>
        <tr>
            <td>Protocol</td>
            <td>string</td>
            <td>"Raw"</td>
            <td>Supported protocols from list above</td>
            <td>❌</td>
            <td>Determines how packets are formatted and split. See below for supported protocols. cnlohr's version only supports "Raw".</td>
        </tr>
        <tr>
            <td>IP</td>
            <td>string</td>
            <td>127.0.0.1</td>
            <td>Valid IPs</td>
            <td>✅</td>
            <td>The IP to send the packets to.</td>
        </tr>
        <tr>
            <td>Port</td>
            <td>int</td>
            <td>7777</td>
            <td>0~65535</td>
            <td>✅</td>
            <td>The port to send the packets to.</td>
        </tr>
        <tr>
            <td>LEDPattern</td>
            <td>string</td>
            <td>"RGB"</td>
            <td>Any valid pattern</td>
            <td>❌</td>
            <td>The order in which to send data for each LED. Any combination of characters R, G, B, Y is valid, in any order, including repetition. Number of characters determines how many bytes each LED takes up in the packet.</td>
        </tr>
        <tr>
            <td>ZigZag</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>❌</td>
            <td>Whether to reverse every odd line of the output for zig-zag wired LED arrays.</td>
        </tr>
        <tr>
            <td>Mirror</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>❌</td>
            <td>Whether the LED matrix wiring runs left-to-right (false) or right-to-left (true).</td>
        </tr>
        <tr>
            <td>RotatedArray</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>❌</td>
            <td>Rotates the LED data 180 degrees for an upside-down matrix.</td>
        </tr>
        <tr>
            <td>SizeX</td>
            <td>int</td>
            <td>1</td>
            <td></td>
            <td>❌</td>
            <td>How wide the LED matrix is. Leave as default if you're not using a matrix, otherwise specify correctly.</td>
        </tr>
        <tr>
            <td>SizeY</td>
            <td>int</td>
            <td>1</td>
            <td></td>
            <td>❌</td>
            <td>How tall the LED matrix is. Leave as default if you're not using a matrix, otherwise specify correctly.</td>
        </tr>
        <tr>
            <td>StartIndex</td>
            <td>int</td>
            <td>0</td>
            <td>0~</td>
            <td>✅</td>
            <td>Where in the visualizer data to start reading when putting data into the packet. Use this if you want the packet to only contain a subset of the visualizer data.</td>
        </tr>
        <tr>
            <td>EndIndex</td>
            <td>int</td>
            <td>-1</td>
            <td>-1~</td>
            <td>✅</td>
            <td>Where in the visualizer data to stop reading when putting data into the packet. Use this if you want the packet to only contain a subset of the visualizer data. -1 means read to the end, regardless of data amount.</td>
        </tr>
        <tr>
            <td>Enable</td>
            <td>bool</td>
            <td>true</td>
            <td></td>
            <td>✅</td>
            <td>Whether to use this output.</td>
        </tr>
    </tbody>
</table>

## Configuring an LED Matrix
If you are outputting to an LED matrix, there are several options available to help you make sure the output is correct.  
{{< alert icon="❗" text="If you use these options, you <b>must</b> specify SizeX and SizeY options correctly as well." />}}

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

<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">Option Name</th>
            <th scope="col">Type</th>
            <th scope="col">Default</th>
            <th scope="col">Range</th>
            <th scope="col"><a href="/docs/general/gettingstarted/#controllability">Controllable</a></th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>PaddingFront</td>
            <td>int</td>
            <td>0</td>
            <td>0~1000</td>
            <td>✅</td>
            <td>Number of padding bytes to append to the front of the packet.</td>
        </tr>
        <tr>
            <td>PaddingBack</td>
            <td>int</td>
            <td>0</td>
            <td>0~1000</td>
            <td>✅</td>
            <td>Number of padding bytes to append to the back of the packet.</td>
        </tr>
        <tr>
            <td>PaddingContent</td>
            <td>int</td>
            <td>0</td>
            <td>0~255</td>
            <td>✅</td>
            <td>What data to put in the padding bytes at the start and end, if present.</td>
        </tr>
    </tbody>
</table>

### TPM2.net

<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">Option Name</th>
            <th scope="col">Type</th>
            <th scope="col">Default</th>
            <th scope="col">Range</th>
            <th scope="col"><a href="/docs/general/gettingstarted/#controllability">Controllable</a></th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>PaddingContent</td>
            <td>int</td>
            <td>0</td>
            <td>0~255</td>
            <td>✅</td>
            <td>What data to put in the padding bytes at the start and end, if present.</td>
        </tr>
        <tr>
            <td>MaxPacketLength</td>
            <td>int</td>
            <td>-1</td>
            <td>-1~65535</td>
            <td>❌</td>
            <td>The maximum size of packets before splitting. -1 means use protocol specified limit (1490).</td>
        </tr>
        <tr>
            <td>ConstantPacketLength</td>
            <td>bool</td>
            <td>false</td>
            <td></td>
            <td>❌</td>
            <td>Whether to make all packets the same size, filling blank space with PaddingContent.</td>
        </tr>
    </tbody>
</table>

### E1.31 (sACN)

Note that the maximum size of data in E1.31 is 512 bytes, which for RGB mode translates to 170 LEDs.

<table class="table table-dark">
    <thead class="thead-dark">
        <tr>
            <th scope="col">Option Name</th>
            <th scope="col">Type</th>
            <th scope="col">Default</th>
            <th scope="col">Range</th>
            <th scope="col"><a href="/docs/general/gettingstarted/#controllability">Controllable</a></th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Universe</td>
            <td>int</td>
            <td>1</td>
            <td>1~63999</td>
            <td>✅</td>
            <td>The DMX universe to address packets to.</td>
        </tr>
        <tr>
            <td>UUID</td>
            <td>hex string</td>
            <td>9E917B13714044CFB46F7A8298692DE3</td>
            <td>any 16-byte hex string</td>
            <td>✅</td>
            <td>Unique sender ID. Change this if you have multiple instances of ColorChord.NET on the network.</td>
        </tr>
    </tbody>
</table>