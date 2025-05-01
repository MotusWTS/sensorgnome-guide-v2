# Introduction and Overview

In this document you will find how to:

* assemble Sensorgnome hardware
* install the Sensorgnome V2 software
* connect to a running Sensorgnome and perform maintenance tasks
* troubleshoot when things don't work

{% hint style="info" %}
**This version of the Sensorgnome user guide is for the "new V2 software" available starting in 2022, that runs on Raspberry Pi4, Pi3, Zero-2W and SensorStation V1.**

If the "Web UI" of your Sensorgnome has multiple tabs with a red accent color it uses the new software. If it is a single web page with little formatting it is the old software.

If you are using the V1 software, refer to the [Sensorgnome user guide for the V1 software](https://docs.motus.org/sensorgnome/v1/) or upgrade to V2.

For updates on the status please refer to the [Motus Community Forum](http://community.motus.org)
{% endhint %}

## Getting help

* Avoid getting frustrated: if it doesn't work, double-check, try again, perhaps try an alternative, but then ask for help! Don't waste hours of time.
* If you can, connect your Sensorgnome to the internet and leave it running & connected: most of time the best troubleshooting happens using the log files it uploads.
* To get help:
  * Search or post on the [Motus Community Forum](http://community.motus.org)
  * Post in the Motus slack (sensorgnome channel)
  * Email tve at voneicken point com
* Things that really help us help you:
  * Screen shots
  * Info about your hardware
  * Having your SG connected to the internet
  * Sensorgnome ID (or the hostname used during configuration)
  * Motus project number and station ID and/or deployment  ID (if applicable)
  * Log files using the web UI or `/var/log/syslog` and `/var/log/sg-control.log` grabbed via SSH

## What is a Sensorgnome?

A Sensorgnome is an automated radio receiver, designed to detect and record radio signals transmitted by wildlife tracking tags, without the need for any person to be present.&#x20;

At its core, a Sensorgnome is powered by a **Raspberry Pi (RPi)**. The RPi runs the software that listens for and records the radio data picked up by the antennas. In addition to the RPi, a Sensorgnome will have one or more USB dongles -- "software-defined radios" -- that take the raw radio signals from the antennas and convert it into a digital form that can be recognized and recorded by the RPi. Finally, the Sensorgnome will include a GPS and power supply, all of which is typically housed in a heavy-duty plastic case.

{% hint style="info" %}
Throughout this document, we will often refer to a Sensorgnome as an **SG**, and to the Raspberry Pi as **RPi**.
{% endhint %}

## About this guide

This guide is divided into four sections:

* Hardware components and configuration, i.e. misc information about radios, USB, HATs, etc.
* Initial software installation and configuration, i.e., how to get started
* Data download and station maintenance, i.e., checking things when on-site
* Appendix with additional information

## Initial deployment checklist

1. Install the software, configure Sensorgnome password, verify access via hot-spot or internet.
2. Verify detection of GPS and optional hardware button/LED.
3. Configure radios and their ports, verify operation using test tags.
4. Verify network configuration and file upload, alternatively verify downloading files to phone/laptop.
5. Create/verify connection of Sensorgnome to Motus receiver deployment.
6. Perform an end-to-end check from test tags to uploaded data.
