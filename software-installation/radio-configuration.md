# Radio Configuration

Once the Sensorgnome operating system is up and running the various radio dongles need to be plugged in, configured, and their proper operation verified.

The configuration consists of mapping each radio to a "port" such that Motus can exactly identify which antenna received a tag transmission.

The verification consists of validating that a test tag transmission is detected, sent to Motus, and classified correctly.

## Radio port configuration

The purpose of USB port mapping is to establish a clean correspondence between (human-readable) labels on USB ports, port numbers reported to Motus, and the port designations used by the Sensorgnome software.

{% hint style="info" %}
While establishing the port mapping may seem like a detail it is actually extremely important because it is the most critical link in communicating antenna orientation to Motus. When detection data is evaluated, the antenna information is often used to determine animal travel direction. If antennas, radios or ports are misidentified the result is bad science!
{% endhint %}

### Background

Motus identifies ports using single digit integers from P1 through P9 and P0 (which stands for 10). It is recommended to physically label the ports on a Sensorgnome in the same way from 1 through 10 (e.g. using marker pen, sticky labels, etc).

The Operating System uses an entirely different port numbering, which consists of the _path_ through ports and hubs to reach a device. For example, a radio plugged into port 3 of a hub, itself plugged into port 4 of an rPi ends up with the path `1.4.3` (the leading 1 refers to the first root hub). We need to establish a mapping from these paths to port numbers 1..10.

The Motus conventions for the ports depend on the device:

* **rPi3B:** port 1 is the upper port next to the Ethernet jack, port 2 is below, 3 is the upper outer port, and 4 is the lower outer port. Typically a Sensorgnome uses a USB hub plugged into port 4 (bottom outer port) and the hub's ports are numbered 4 through 7 or 4 through 10 depending on whether it's a 4-port or a 7-port hub.
* **rPi4B:** the numbering should be reversed so the USB hub is plugged into a USB3 port (the blue ports): port 1 is the lower outer port, port 2 above it, port 3 the lower port next to the ethernet jack, and port 4 above it. Plug a hub into this last port, i.e. top blue USB3 port.
* **rPi Zero-2W:** there is only one micro-USB port and the port numbering will depend on the type of hub used.
* **SensorStation V1:** the built-in CTT radios at the bottom of the board are ports 1-5, the USB connectors labeled USB1 through USB5 are ports 6-10, USB6 & USB7 should not be used for radios (plug the WiFi dongle into one of them and a cellular modem into the other).

### Steps

To perform the mapping follow these steps:

* Ensure all USB ports are labeled and all devices to be plugged in are also labeled.
* Bring up the Sensorgnome's web UI and locate the "Port Mapping" section on the Radios tab.
* Unplug all USB devices (leaving any hub plugged in).
* Plug one device into the desired port,
* Watch the device appearing in the Web UI's "Devices" widget, note the port path shown as well as the port selected. If it's not red, then it's probably OK.
* To change the port selected, edit the Port Mapping (pencil icon at the top), edit the mapping and save it.
* Plug each remaining USB device in one at a time and repeat.

The port mappings are deterministic when using the same model rPi and the same hub models, so the port mapping can be copied between identical devices.

In the end it is highly recommended to double check the correct port mappings:

* Verify in the Web UI "Devices" that all devices are assigned a port 1 through 10 (no port should be shown in red)
* Unplug each device in turn, watch the correct line disappear, plug it back in and watch the correct line with the correct port number reappear.

## Verifying successful reception

To verify the correct end-to-end operation of radios some test tags are necessary. For Lotek tags a tag database with the test tags is recommended but not essential.

### Verify the reception of Lotek tags

* Ensure at least one FunCube or RTLSDR radio is plugged in and shows in the Web UI Devices section with a port numbered 1 through 10 (i.e. not shown in red).
* Ensure the test tag is activated.
* On Web UI's radio tab watch the "Detection log" widget in the Web UI, when the tag transmits next you should see 3-4 lines of the form\
  `PLS: p5,1665900923.5965,3.96,-48.31,-60.86`\
  `PLS: p5,1665900923.6185,3.948,-48.76,-60.92`\
  `PLS: p5,1665900923.6380,3.986,-47.99,-60.94`\
  `PLS: p5,1665900923.6625,3.989,-47.62,-61.01`\
  PLS stands for pulse, p5 refers to "pulse on port 5", this is followed by the time-stamp, the frequency offset in kHz from the nominal frequency, the RSSI (dBm) and the noise (dBm).
* It takes 2 bursts of 4 pulses to detect a tag if the tag is found in the local tag database. A tag detection is hown as follows: TAG: \
  &#x20;`00:01:14.346 p3 2.812 kHz -39.57 / -53.8 dB` where the `p3` refers to USB port 3
* If you loaded a tag database and the tag is in that database then after 2 radio bursts (i.e. 8 PLS lines) you should see a tag detection in the log:\
  `L5,1665900923.5965,TestTags#1.1@166.38:25.1,3.971,0.02,-48.1,11.1,-60.9,3223,95,1.97e-07,-4.96e-05,166.38`\
  The L5 refers to a Lotek tag detected by the radio on port 5. This is followed by the tag name and ID, the nominal frequency, the pulse interval (25.1s in this case), the freq offset in kHz, and further details about the pulse burst.
* Note that if you have multiple radios plugged in you should see live pulses and live known tags from all of them, so you should see more than 4 lines appear at once.

### Verify the reception of CTT tags

* Ensure at least one CTT MOTUS adapter or equivalent is plugged in and shows in the Web UI Devices section with a port numbered 1 through 10 (i.e. not shown in red).
* Ensure the test tag is activated, or in the case of a solar LifeTag that it has bright light shining on it.
* On Web UI's radio tab watch the "Detection log" widget and you should see tag transmissions appear of the form\
  `TAG: T6,1665900910.226,78664C33,-95`\
  The T6 refers to CTT radio on port 6, 1665900910.226 is the timestamp (seconds since 1970-1-1) 78664C33 is the CTT tag ID and -95 is the RSSI (dBm).

## Troubleshooting

* As you plug radios into USB ports they should appear in the Web UI Devices section within 2-3 seconds. If they don't, reboot the Sensorgnome. If that doesn't make them show up there is a software or hardware problem. Compare with what happens as you plug/unplug other identical devices if you have any.
* ~~If a FUNcube is shown in the Devices section with a frequency other than the expected one (166.376 for north America) press the "Refresh Devices List" button. If that doesn't fix it _TBD_~~
* If the Devices look correct but the Live Pulses do not show anything first ensure your test tag is active. Some test tag intervals are long, e.g. 25 seconds, so have some patience. If nothing happens, reboot the Sensorgnome.
* If you see pulses but no tag detections re-upload your tag database and reboot.
