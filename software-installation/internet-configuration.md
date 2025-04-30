# Internet configuration

Wherever possible, SG's should be connected to the Internet for three benefits:

* Automatically upload data to Motus, resulting in more up-to-date data, fewer trips to visit a station, and more timely identification (and resolution) of any issues with the receiver.
* Remote monitoring of the SG's operation, including radio operation, SD-card usage, upload progress, detections, etc.
* Remote administration of the SG to perform necessary upgrades, enable/disable the hot-spot, reboot, etc.

## Option 1: Wired internet over Ethernet

The simplest method of syncing data over the internet is to plug the RPi into the Internet via an Ethernet cable. If the internet is not password-protected and the SG can obtain an address via DHCP the SensorGnome should connect to the Motus server within minutes and begin syncing data.&#x20;

### Steps:

1. Connect an Ethernet cable that is non-password-protected to the SG.
2. Confirm that the SG has connectivity in its Web UI or on the Sensorgnome server.

## Option 2: WiFi Client

Connecting over a WiFi network is also an option by configuring the network's SSID and typ. password in the SG's web UI.

{% hint style="warning" %}
The RPi3 only supports the 2.4Ghz band, not the 5Ghz band. The RPi4 supports both bands.
{% endhint %}

### **Steps:**

1. Connect to the Sensorgnome's web UI, e.g. using its hot-spot.
2. On the network tab enter the WiFi's SSID and password into the WiFi Client widget (and click the green checkmark). (Changing the country code from 00 to your country's two letter ISO code, e.g. US, CA, DE, et. is optional and unlikely to change anything, code 00 is a "lowest common denominator".)
3. Confirm that the SG connects to the network and obtains connectivity to motus.org.

{% hint style="info" %}
On the RPi3 and RPi4 the WiFi hot-spot functionality is independent of the WiFi client functionality and both can be active at the same time. However, the hot-spot may temporarily disconnect and reconnect when configuring the WiFi client because it may have to switch channel as it must use the channel dictated by the WiFi client connection.
{% endhint %}

## Option 3: Cellular

Cellular connectivity is supported using the Sixfab 4G/LTE cellular modem kit/HAT as well as the Waveshare SIM7600 series USB "dongles".

Simply installing the HAT or plugging the SIM7600 device in _should_ cause everything to magically function. In the case of the Waveshare device please purchase a simple (passive) GPS antenna with a u.Fl connector and plug it into the GPS port (you have to open the device as shown in the instructions provided by Waveshare).

More information forthcoming...

## Automatic uploads

When the Sensorgnome connects to a WiFi, Ethernet, or cellular network it expects to obtain an IP address for itself and the address of a default gateway via DHCP. This is standard DHCP configuration but of course can be disabled/changed by the administrator of the local network.

Whenever the Sensorgnome has a default gateway it:

* displays "internet via xxx" in the "internet via" widget on the network tab, where xxx designates the type of interface
* checks general internet connectivity and displays the result in the "Internet"  widget
* checks connectivity to motus.org and displays the result in the "Motus.org" widget
* obtains an upload authentication token from a Sensorgnome server
* uploads data files to motus.org as soon as they are complete, i.e., the next file is started, which typ. happens once an hour
* uploads any old data files that have not been previously uploaded

### Known issue: authentication

Authentication with motus.org is necessary to perform the uploads and the exact mechanism is still in flux. Currently three mechanisms are supported, they all "get the data to where it needs to go" but have issues. Motus server back-end functionality needs to be enhanced to remove these issues.

Note: option 1 is recommended, although you may want to implement option 2. Option 3 was coded first in the Sensorgnome software and remains available until final resolution of the authentication mechanism.

#### Option 1: automatic token

If you do nothing, the SG obtains a token from a Sensorgnome server and uploads data as user `sg-uploader` into Motus project 460. You will not be able to see the upload jobs in your project but you can verify that they occur by looking at the receiver. (Details TBD.)

#### Option 2: give sg-uploader access to your project

If you start with option 1 you can, at any time, add the `sg-uploader` user to your project and give it "update" access only to "data", all other permissions can be none:

![](<../.gitbook/assets/image (12).png>)

The result of this is that the upload jobs will be part of your project and can be seen like any manual data uploads.

#### Option 3: enter your Motus creds

You can also enter your Motus login credentials into the SG web UI on the files tab. This causes the SG to obtain an authentication token directly from motus.org and use that to upload files. Your login user/password are not stored anywhere, only the token is. The token is periodically refreshed and remains valid for 45 days if the SG cannot connect to the internet for such a refresh. If it times out, the SG reverts to option 1 until Motus credentials are again entered.
