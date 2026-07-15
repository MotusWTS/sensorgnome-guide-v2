# Introduction and Overview

In this document you will find how to:

* assemble SensorGnome hardware
* install the SensorGnome V2 software
* connect to a running SensorGnome and perform maintenance tasks
* troubleshoot when things don't work

{% hint style="info" %}
**This version of the SensorGnome user guide is for the "new V2 software" available starting in 2022, that runs on Raspberry Pi4, Pi3, Zero-2W and SensorStation V1.**

If the "Web UI" of your SensorGnome has multiple tabs with a red accent color it uses the new software. If it is a single web page with little formatting it is the old software.

If you are using the V1 software, refer to the [SensorGnome user guide for the V1 software](https://docs.motus.org/sensorgnome/v1/) or upgrade to V2.

For updates on the status please refer to the [Motus Community Forum](http://community.motus.org)
{% endhint %}

## Getting help

* Avoid getting frustrated: if it doesn't work, double-check, try again, perhaps try an alternative, but then ask for help! Don't waste hours of time.
* If you can, connect your SensorGnome to the internet and leave it running & connected: most of time the best troubleshooting happens using the log files it uploads.
* To get help:
  * Search or post on the [Motus Community Forum](http://community.motus.org)
  * Post in the Motus slack (sensorgnome channel)
  * Email tve at voneicken point com
* Things that really help us help you:
  * Screen shots
  * Info about your hardware
  * Having your SG connected to the internet
  * SensorGnome ID (or the hostname used during configuration)
  * Motus project number and station ID and/or deployment ID (if applicable)
  * Log files using the web UI or `/var/log/syslog` and `/var/log/sg-control.log` grabbed via SSH

## What is a SensorGnome?

A SensorGnome is an automated radio receiver, designed to detect and record radio signals transmitted by wildlife tracking tags, without the need for any person to be present.

At its core, a SensorGnome is powered by a **Raspberry Pi (RPi)**. The RPi runs the software that listens for and records the radio data picked up by the antennas. In addition to the RPi, a SensorGnome will have one or more USB dongles -- "software-defined radios" -- that take the raw radio signals from the antennas and convert it into a digital form that can be recognized and recorded by the RPi. Finally, the SensorGnome will include a GPS and power supply, all of which is typically housed in a heavy-duty plastic case.

{% hint style="info" %}
Throughout this document, we will often refer to a SensorGnome as an **SG**, and to the Raspberry Pi as **RPi**.
{% endhint %}

## About this guide

This guide is divided into three sections:

* Setup and operation: Initial software installation, configuration, data downloading, etc
* Hardware components: misc information about radios, USB, HATs, how to assemble an SG
* Appendix with additional information, resources, and troubleshooting tips

## Initial deployment checklist

1. Install the software and configure the SensorGnome password
2. Connect to your SensorGnome
   1. verify GPS and time source
   2. Configure radios and their ports, verify operation using test tags.
   3. Configure / verify network configuration&#x20;
3. Create new station on Motus.org and associate this SG with it. (See [here](https://app.gitbook.com/s/jf0K5m6GrB75xw7Xweac/stations/station-metadata) for more about station metadata management.)
4. Perform an end-to-end check from test tags to uploaded data.
