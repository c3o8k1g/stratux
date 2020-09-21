# Stratux - European edition
This is a fork of the original Stratux version, incorperating many contributions by the community to create a
nice, full featured Stratux-OGN image that works well for europe.

## Disclaimer
This repository offers code and binaries that can help you to build your own traffic awareness device. We do not take any responsibility for what you do with this code. When you build a device, you are responsible for what it does. There is no warrenty of any kind provided with the information, code and binaries you can find here. You are solely responsible for the device you build.

## Main differences to original Stratux
* Original Stratux: https://github.com/cyoung/stratux
* Added OGN receiver functionality to receive several protocols on the 868Mhz frequency band, comparable to what the OpenGliderNetwork does
* Merged VirusPilot's fixes and improvements for U-Blox 8/9 devices and Galileo/Glonass reception ([link](https://github.com/VirusPilot))
* If no pressure sensor is present, Stratux EU will try to estimate your pressure altitude with atmospheric information received from other aircraft. We still recommend using some kind of barometric sensor (e.g. Stratux AHRS module). More information can be found [here](https://github.com/b3nn0/stratux/wiki/Altitudes-in-Stratux-EU)
* By default, OGN and DeveloperMode is enabled, UAT is disabled
* Merged Stratux Web-Radar for web-based traffic display by [TomBric](https://github.com/TomBric)
* Added a map to the web interface that shows received traffic
* Upgraded the RaspberryPi Debian system to the latest debian packages (RaspiOS Buster)
* Hide Weather/Towers page if UAT is disabled
* Added a special "Skydemon wonky GDL90 parser" workaround to reduce Skydemons constant detection of very short disconnects (see below)
* Support for NMEA output (including PFLAA/PFLAUU traffic messages) via TCP Port 2000 and [serial](https://github.com/b3nn0/stratux/wiki/Stratux-Serial-output-for-EFIS's-that-support-GDL90-or-Flarm-NMEA-over-serial)
* Estimation of Mode C/S target distance by signal strength, transmission of bearingless targets via NMEA and GDL90
* Support for changing the Stratux's IP address
* Possibility to enter multiple ownship transponder HEX codes, Stratux will automatically decide which of these are actually you. This is useful if you have multiple aircraft that you regularly fly with (e.g. add all club aircraft)
* X-Plane 11 compatible output for EFBs that support simulator input (experimental, unsupported. Might make it possible to connect Garmin Pilot). Based on original work by 0x74-0x62
* Support for external devices that provide NMEA with traffic, such as OGN Tracker, SoftRF or potentially a FLARM mouse (only tested with some devices. Your milage may vary)
* Support for WiFi Direct connection to make it possible to let Android have mobile data connection while connected to the Stratux
* Support for up to three SDRs
* Support for the RaspberryPi 3B+ and 4B
* Many more smaller tweaks all over the place

## Building the Europe Edition
Building the european Edition is practically the same as the official Stratux. More information can be found here:
http://stratux.me/
You can also buy a prebuilt unit.
Notable however: Stratux recently started selling a new "Stratux V3 UAT Radio" for UAT reception. This radio does NOT work for OGN reception, so make sure you get the old V2 radios instead or get a secondary RTLSDR dongle from somewhere else.
Also, it is recommended to purchase a 868 Mhz antenna for OGN reception. The standard 978 Mhz antenna can receive OGN targets, but the range will be limited.
Additionally, you will need a PC with an SD Card reader.
Download the latest image [here](https://github.com/b3nn0/stratux/releases)
and use an arbitrary tool to burn the image to your Micro SD Card (e.g. "Etcher", see [here](https://www.raspberrypi.org/documentation/installation/installing-images/)).
A summary of supported hardware, which can be used as a buying guide, can be found [here](https://github.com/b3nn0/stratux/wiki/Supported-Hardware).



## Notes to SkyDemon Android/iOS Users
SkyDemon is probably the most popular EFB in Europe, and we are trying hard to make Stratux work as good as possible in SkyDemon, which is not always easy. Most notably, with original Stratux on a RaspberryPI 2b, you can often observe disconnects, which will show as many red dots in your track log.

Thorough analysis has shown that this is caused by a mix of
- RaspberryPI's brcmfmac wifi driver and its behaviour when UDP package delivery is slow
- Androids/iOSs handling of UDP packets under load - namely the fact that it will delay them
- A wonky GDL90 implementation in SkyDemon (which is not very error tolerant, even though the UDP RFC explicitly says that applications should expect errors and work around them).

If you will suffer from these problems depends on many factors, but it is certainly possible.
The real solution would be, that SkyDemon behaves more error tolerant, but they seem to be resiliant to do so.
As of version 1.5b2-eu004, the web interface has a settings switch labeled "SkyDemon Android disconnect bug workaround". Enabling this will cause Stratux to send position reports to the EFB every 150ms instead of every second.
Experiments show that SkyDemon handles this relatively well and will show disconnects much rarer.
Note that this is an ugly hack and does not conform the GDL90 specification, but it seems to do the job for SkyDemon.


