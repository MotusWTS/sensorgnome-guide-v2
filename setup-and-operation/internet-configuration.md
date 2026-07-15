# Internet configuration

Wherever possible, SG's should be connected to the Internet for three benefits:

* Automatically upload data to Motus, resulting in more up-to-date data, fewer trips to visit a station, and more timely identification (and resolution) of any issues with the receiver.
* Remote monitoring of the SG's operation, including radio operation, SD-card usage, upload progress, detections, etc.
* Remote administration of the SG to perform necessary upgrades, enable/disable the hot-spot, reboot, etc.

## Option 1: WiFi&#x20;

The SensorGnome can connect to any standard WiFi network using the Raspberry Pi's built-in WiFi client. This is accessed and configured on the SG's web interface.&#x20;

{% hint style="warning" %}
The RPi 3 only supports the 2.4 Ghz band, not the 5 Ghz band. The RPi 4 supports both bands.
{% endhint %}

{% stepper %}
{% step %}
### Connect to the SensorGnome's web interface

&#x20;See [Connecting to your SensorGnome](connecting-to-your-sensorgnome.md) if you need a refresher.
{% endstep %}

{% step %}
### On the `NETWORK` tab, click on the "pencil" icon in the WiFi Client widget to edit the configuration

<figure><img src="../.gitbook/assets/image (42).png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Enter the WiFi network name and password

Enter the WiFi's SSID (the WiFi network name) and passphrase (password) and click the green check-mark to save.

If there is no password, leave that field blank (you will still see the `*******` placeholder when it's blank). Changing the country code from 00 to your country's two letter ISO code, e.g. US, CA, DE, et. is optional and unlikely to change anything; code 00 is a "lowest common denominator".
{% endstep %}

{% step %}
### After several seconds, the Network Connectivity widget should indicate connectivity

<figure><img src="../.gitbook/assets/image (43).png" alt="" width="563"><figcaption><p>This web interface will confirm if you are connected</p></figcaption></figure>

{% hint style="info" %}
On the RPi3 and RPi4 the WiFi hotspot functionality is independent of the WiFi client functionality and both can be active at the same time. However, the hot-spot may temporarily disconnect and reconnect when configuring the WiFi client because it may have to switch channel as it must use the channel dictated by the WiFi client connection.
{% endhint %}
{% endstep %}
{% endstepper %}

## Option 2: Wired internet over Ethernet

The simplest method of syncing data over the internet is to plug the RPi into the Internet via an Ethernet cable. If the internet is not password-protected and the SG can obtain an address via DHCP the SensorGnome should connect to the Motus server within minutes and begin syncing data.

{% stepper %}
{% step %}
### Connect an Ethernet cable that is non-password-protected to the SG
{% endstep %}

{% step %}
### Confirm that the SG has connectivity in its Web UI&#x20;

[Connect to your SensorGnome](connecting-to-your-sensorgnome.md) then access the `NETWORK` tab to check the current status

<figure><img src="../.gitbook/assets/image (37).png" alt="" width="563"><figcaption><p>The SG web interface will indicate whether an internet connection exists, as well as the particular type, in this case Ethernet.</p></figcaption></figure>
{% endstep %}
{% endstepper %}

## Option 3: Cellular

Cellular connectivity is supported using the[ SixFab 4G/LTE cellular modem kit/HAT](https://sixfab.com/product/raspberry-pi-4g-lte-modem-kit/) as well as the Waveshare SIM7600 series USB dongles.

Simply installing the HAT or plugging the SIM7600 device in _should_ cause everything to magically function, provided the SIM you are using is activated and has a valid data plan. The SixFab modem kit has an integrated GPS, though in the case of the Waveshare device you'll need to purchase a simple (passive) GPS antenna with a u.Fl connector and plug it into the GPS port (you have to open the device as shown in the instructions provided by Waveshare).

<figure><img src="../.gitbook/assets/image (19).png" alt="" width="563"><figcaption></figcaption></figure>

If you are experiencing issues with cell connectivity, please refer to the [Troubleshooting ](../appendix/cell-modem-troubleshooting.md)section.

## SG Hub

SG Hub, at [**www.sensorgnome.net**](https://www.sensorgnome.net/), provides a convenient way to check on an internet-connected SensorGnome. Along with the most recent check-in time, there are a variety of other diagnostic and status plots to help asses the "health" of the various SensorGnomes.

Generally, within a minute or two of connecting for the first time, a SensorGnome will show up in the "Recently installed Sensorgnomes" list. You can sort this list by any of the columns, and click on any of the SGs for more detailed info about that particular SG.

<figure><img src="../.gitbook/assets/image (41).png" alt="" width="563"><figcaption><p>SG Hub will show recently connected SG</p></figcaption></figure>

## Automatic upload process

When the SensorGnome connects to a WiFi, Ethernet, or cellular network it expects to obtain an IP address for itself and the address of a default gateway via DHCP. This is standard DHCP configuration but of course can be disabled/changed by the administrator of the local network.

Whenever the SensorGnome has a default gateway it:

* displays "internet via xxx" in the "internet via" widget on the network tab, where xxx designates the type of interface
* checks general internet connectivity and displays the result in the "Internet" widget
* checks connectivity to motus.org and displays the result in the "Motus.org" widget
* obtains an upload authentication token from a SensorGnome server
* uploads data files to motus.org as soon as they are complete, i.e., the next file is started, which typically. happens once an hour
* uploads any old data files that have not been previously uploaded
