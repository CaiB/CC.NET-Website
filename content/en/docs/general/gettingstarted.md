---
title: "Getting Started"
description: "ColorChord.NET requires a little bit of configuration, but it's worth it!"
lead: "ColorChord.NET requires a little bit of configuration, but it's worth it!"
draft: false
images: []
menu: 
  docs:
    parent: "general"
weight: 110
toc: true
---

Thanks for taking the time to check out ColorChord.NET. I hope you enjoy it, and please feel free to let me know if you encounter any issues or have any feedback. One way is to [open an issue on GitHub](https://github.com/CaiB/ColorChord.NET/issues).

## Installation

#### .NET Runtime
The .NET Runtime is needed in order to run ColorChord.NET. Currently, ColorChord.NET is built against **.NET 5**. There are 2 versions: The **.NET Runtime**, which is required, and the .NET SDK, which is larger, and is only needed if you want to develop .NET software. The SDK includes the Runtime as well.

You can download the .NET 5 Runtime from Microsoft here, it works on Windows, Linux, and macOS:  
<a class="btn btn-primary btn-lg px-4 mb-2" href="https://dotnet.microsoft.com/download/dotnet/current/runtime" role="button">Download .NET Runtime</a>

#### ColorChord.NET
Once you have the .NET Runtime installed, you'll need to download ColorChord.NET. You will have 2 choices:
- **Autobuild:** This contains the latest under-development additions, changes, and bugfixes. However it may not be stable or ready for general use yet.
- **Release:** This contains a reasonably stable build, without the bleeding-edge development work.

Choose one, then expand the "Assets" section, and download the .ZIP file (not the source code) from GitHub below:  
<a class="btn btn-primary btn-lg px-4 mb-2" href="https://github.com/CaiB/ColorChord.NET/releases" role="button">Download ColorChord.NET</a>

Installing ColorChord.NET is simple.
1) Extract the .zip file to a convenient location (e.g. Your user account's Documents)
2) Start the application, depending on your platform:  
  a) Windows: Double-click on ColorChord.NET.exe  
  b) Linux: Run "dotnet ColorChord.NET.dll"
3) On first run, it will auto-generate the default config.json file.
4) Exit ColorChord.NET.
5) Edit the config file to suit your needs (see below).
6) Run ColorChord.NET again after editing the config, check the console for any warnings or errors, and enjoy!

## Configuration

There are 5 types of components in ColorChord.NET:
- Audio Sources: Brings audio data from your system into ColorChord.NET.
- Note Finder: Turns raw audio data into note information.
- Visualizers: Takes note info from the Note Finder, and transforms it into something interesting to look at, ready for output.
- Outputs: Takes data from a visualizer, and actually displays/outputs it somewhere.
- Controllers: Edits the behaviour of any of the above components while ColorChord.NET is running.

A single instance of the application supports a single audio source and Note Finder, any number of visualizers, each with its own set of (any number of) outputs. This allows for a single audio stream to be processed and displayed in almost any desired combination of ways.

#### Controllability

Many components in ColorChord.NET have "controllable" config options, as seen in the tables in this documentation. This does not refer to whether you can change the setting in the config file (anything that is listed in the docs can be changed by you), but rather refers to whether a *controller* component is able to change this setting while ColorChord.NET is already running.

#### Config File Notes

- You can see a [sample config file here](https://github.com/CaiB/ColorChord.NET/blob/master/ColorChord.NET/sample-config.json). This is what the program auto-generates if no config is found.
- If an option is not specified in the configuration file, the default value is used.
- If an unrecognized option or invalid value is specified, a warning is output to the console. Always check for these after modifying the config in case you made a spelling mistake.
- You can choose a different configuration file by running the program with the command line option "config YourFile.json".
- Range specifies the set of input values that can be used. Extreme values may not make any sense in practice though, so make small changes from the defaults to start. Range is just specified to prevent completely invalid input.