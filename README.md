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

## LabVIEW API Usage

LabVIEW API has some built VI for following versions:
* x64:
  - 2010
  - 2016
  - 2021 SP1
* x86:
  - 2010

If you don't see your LabVIEW version here, you can download any one which is ≥ than 2021 SP1 for x64 and ≥ than 2010 for x86 respectively.

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
