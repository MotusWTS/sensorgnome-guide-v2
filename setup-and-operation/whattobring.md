---
description: >-
  A checklist of items and software to have on hand when working with a
  SensorGnome in the field.
---

# Checking a SensorGnome in the field

Before you visit a SensorGnome, it's important to have a few key items and software on hand or on your computer. Some of these are absolutely essential while others can save you time and headaches in the future if you already have them with you.

## What to bring

1. A **web browser** device: either a laptop with WiFi or a smartphone (or laptop with Ethernet and cable)
2. The SensorGnome's **password** (SensorGnomes have one single password)
3. Location of any hotspot-enable button
4. Network information to connect to the SensorGnome if you will not be using the hotspot (hostname or IP address and local Wifi password, etc)
5. Information about antennas, cables, dongles/radios, and ports: which antenna corresponds to which cable, which dongle it should be connected to, and which port it should be plugged into

### In addition...

* A Lotek test tag and a CTT test tag to perform an end-to-end verification of the system
* An SD-card prepared with the latest version of the SensorGnome software&#x20;

## Connecting to a SensorGnome

The SensorGnome is managed through its web interface which is generally accessed using the SensorGnome's  hotspot. Alternatively, it is possible to use an Ethernet cable or to connect to the building's WiFi if the SensorGnome is connected to it as well.

### Blinking LEDs

If you can see the Raspberry Pi board it is helpful to first check its two LEDs. These are near the power connector (USB cable) and diagonally opposite the Ethernet jack: a red one and a green one. The red LED should be solid on indicating that power is on. The green LED should be flashing 1x, 2x, or 3x every 2 seconds:

* 1x: operating, hotspot off, and no internet access
* 2x:  operating, hotspot ON, no internet access
* 3x: operating, internet access OK (hotspot may be on or off)

### Hotspot connection

Many SensorGnomes keep the Hotspot on 24x7. Open the internet or wifi settings on you laptop or phone where it scans for available networks and locate one with the SensorGnome's ID, i.e. something of the form SG-1234RPI4ABCD.

Some SensorGnomes turn the hotspot off and have a button to press to enable the hotspot. Typically this button is accessible from outside the enclosure.

Connect to that network, enter the password, and use the web browser that typically pops-up.

The hot-spot acts as a "captive portal", which means that it looks to your computer/phone like a WiFi network typical of airports or hotels where you have to enter credentials or accept an agreement to get internet access. However, the SensorGnome never actually provides internet access...

Some of the issues users have encountered are:

* a web browser automatically opens to show the SensorGnome's web UI, but it may be a special system browser and not your regular browser (on iOS that browser seems not to show certain charts)
* your computer/phone may become "impatient" about not getting internet access and decide to switch back to your regular WiFi
* if using Android, your phone may use the cellular network instead of the Hot-Spot and you most likely have to turn cellular/mobile off

In many cases the hot-spot "just works" but being aware of the potential issues helps.

If you get stuck first check that your laptop/phone is still connected to the hot-spot and once it is connect to the SensorGnome using your favorite browser at `http://192.168.7.2` to avoid the system browser.

### Using a WiFi or Ethernet network

Some SensorGnomes may be connected to the local building/campus WiFi or Ethernet in which case you will need to connect your laptop/phone to that network too. You _may_ then be able to access the SensorGnome at http://sgpi.local but in many cases you will need to know the SensorGnome's IP address (like http://192.168.1.1).

Over a WiFi network or Ethernet the SensorGnome uses HTTPS, i.e. encryption. If you access it using HTTP you will see a redirect page that shows a link like:

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>SensorGnome redirect page when accessed using HTTP</p></figcaption></figure>

This link, using `my.local-ip.co` is there to make the certificate check used by HTTPS work. However, if you r SensorGnome is not regularly updated the certificate may be out of date and you may have to jump through hoops to make your browser proceed. (This is why it's typically easier to use the hotspot.)

## Checking the SensorGnome's health

### On the home (leftmost) tab:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* check that the time source is as expected, e.g. NTP (network time protocol) or GPS
* check that the boot time makes sense (some SensorGnomes reboot daily in winter due to power constraints but generally they should remain on all the time)
* check the SD-card usage
* check that the GPS has a fix, unless the SensorGnome doesn't have a GPS. "No-sat" means there is a GPS but it has no fix: that's often a problem

### On the Radios tab

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* Check that the radio number match up with what's installed and that there are no invalid ports
* Check that the ports (in the port mapping, e.g. 1, 5, 6, 7 in this screen shot) are as expected
* If you have test tags with you observe their detection in the panel on the right, here port 5 detects Lotek pulses

## Troubleshooting

* If devices are missing, e.g. GPS shows no-dev or radios that are evidently plugged in are not showing up the first step should be a reboot (on the Software tab).
* ...
