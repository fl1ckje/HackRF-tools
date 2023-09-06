# HackRF-tools
HackRF tools built for OS Windows 10 x64 and newer.

Big thanks to [makar853](https://github.com/makar853) for C# wrapper for LabVIEW [makar853/nethackrf](https://github.com/makar853/nethackrf) repo.

## Installation and usage quick-guide
1. Go to [Releases](https://github.com/fl1ckje/HackRF-tools/releases), choose and download latest release setup binary.
2. Run the executable file and follow setup installation steps.
3. Run any stuff you need from the start menu folder or from installation folder (C:/HackRF). You can also use PowerShell or Command shell (cmd.exe) to execute hackrf-tools (setup binary adds bin folder path variable to PATH in system environment).

## Update quick-guide
1. Uninstall currently installed release.
2. Download newer release setup binary which you do want to install.
3. Run the executable file and follow setup installation steps.

## LabVIEW API information

LabVIEW API has some built VI for following versions:
* x64:
  - 2010
  - 2016
  - 2021 SP1
* x86:
  - 2010

If you don't see your LabVIEW version here, you can download any one which is ≥ than 2021 SP1 for x64 and ≥ than 2010 for x86 respectively. Then just save VIs for your target version.

If you encounter error while loading .NET assembly in LabVIEW, then try doing this:
1. Close LabVIEW if it's running.
2. Use a text editor to create a file that contains the following text:
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup useLegacyV2RuntimeActivationPolicy="true">
        <supportedRuntime version="v4.0.30319" />
    </startup>
</configuration>
```
3. Save the file as LabVIEW.exe.config in the same directory of the LabVIEW.exe file. This directory is typically located in `C:\Program Files\National Instruments\LabVIEW 20xx\` for x64 and in `C:\Program Files (x86)\National Instruments\LabVIEW 20xx\` for x86 respectively.
4. Run LabVIEW and try to work with API or NetHackRF assembly again.

## LabVIEW API Usage
Firstly, you need to get list of connected hackrf devices by using `NetHackrf.HackrfDeviceList()` which returns array of `NetHackrf.hackrf_device_info` objects.

Each `NetHackrf.hackrf_device_info` object has `OpenDevice()` method which returns `NetHackrf` object.

To start receiving or transmitting data you need to run `StartRX()` or `StartTX()` method of `NetHackrf` object which would return `System.IO.Stream` object. Stream object is used to write or read IQ interleaved data.

Hackrf is a half-duplex device thus only one stream can be used at a time. Before using `StartRX()` or `StartTX()` methods again, you should stop the existing stream by using its `Dispose()` method.

You can control transceiver by writing NetHackrf class properties which are:

```cpp
double FilterBandwidthMHz
double CarrierFrequencyMHz
double SampleFrequencyMHz
bool AntPower
bool ClkOut
double LNAGainDb
double VGAGainDb
double TXVGAGainDb
bool AMPEnable
```
