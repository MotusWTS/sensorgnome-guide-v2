# Software installation

The SensorGnome software runs off a MicroSD card. In order to install the software, it is "flashed" to the SD card using an imaging software such as Raspberry Pi Imager. After it is booted for the first time, it needs to be "initialized" by setting a password. The basic sequence is:

* Flashing the MicroSD card with the SensorGnome software
* Initial configuration, where you boot up the SG and set a password&#x20;
* Connect to the SG and access its web interface, where you can confirm that it's running and configured properly. This last step is described in the [next page](connecting-to-your-sensorgnome.md) since it's an important step _whenever_ you visit a station with a SensorGnome, and not just after the software installation.

## Prerequisites

* MicroSD card with at least 16 GB. Try to avoid low quality or old SD cards, and instead spend a bit more money (\~$20) to buy a high quality card such as SanDisk Extreme Pro or Samsung Pro/High Endurance as these will reduce the likelihood of data loss or corruption.
* A computer **AND** a method of reading and writing to a MicroSD card. Most computers do not have MicroSD slots, so a USB adapter is usually the best option.
* The latest **SensorGnome software** release downloaded from [https://www.sensorgnome.net/download](https://www.sensorgnome.net/download). As of 2026-07-14 the latest&#x20;
* **Raspberry Pi Imager** installed on your computer, the latest version can be downloaded from [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/). Other imaging software, such as WinDiskImager or Balena Etcher, work as well.
* **Raspberry Pi** 4B, 3B+, 3B, Zero-2W, or SensorStation V1.

## Flashing the MicroSD card

The process of flashing the MicroSD card is the same if you are installing the SG software for the very first time, or if you are upgrading an existing SG with a _fresh software card_ (as opposed to upgrading over the Internet).

{% stepper %}
{% step %}
### Launch Raspberry Pi Imager

Download from [here](https://www.raspberrypi.com/software/) if you don't yet have it.
{% endstep %}

{% step %}
### Select `Use custom` on the right hand side of the `OS` tab

<figure><img src="../.gitbook/assets/image (39).png" alt="" width="509"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Select the SensorGnome software image&#x20;

Browse to where you saved the SensorGnome software on your computer, select the file, then click `NEXT`. Download the latest SG software from [here](https://www.sensorgnome.net/download) if you don't yet have it.

{% hint style="info" %}
It's best to always have the latest software version on your computer in the event that you need to flash a new SD card. Though you can load the compressed `.zip` file directly into Raspberry Pi Imager, the process will go faster the next time if you extract the file and use the `.img` file.
{% endhint %}
{% endstep %}

{% step %}
### Select the MicroSD as the Storage Device, and click `NEXT`

<figure><img src="../.gitbook/assets/image (44).png" alt="" width="507"><figcaption></figcaption></figure>


{% endstep %}

{% step %}
### Click `WRITE` and then click confirm on the warning dialog

<figure><img src="../.gitbook/assets/image (45).png" alt="" width="501"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Wait for the process to complete

There is both a writing and verification stage and together the entire process can take 10-15 minutes.

<figure><img src="../.gitbook/assets/image (47).png" alt="" width="507"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Move on to [initial configuration](initial-software-installation.md#initial-configuration)

Once the writing and verification is completed, you're now ready for the [initial configuration as described below](initial-software-installation.md#initial-configuration).
{% endstep %}
{% endstepper %}

#### Troubleshooting

Usually the flashing process is straightforward, but if you have issues or the verification fails, you can try the following:

* Try again with no changes. Sometimes this works if, for instance, the MicroSD card reader was jostled while the process was running
* Try a different USB port, or a different laptop if you have one
* Try a different MicroSD card reader.&#x20;
* Try a different MicroSD card. Older cards in particular can be prone to failing
  * If you only have one card, sometimes it helps to delete all partitions and/or format the card before proceeding with the flashing
* If verification fails and you have no alternative, try it in the SG anyway. Sometimes it still works.

#### SensorStation V1

It is possible to flash the SG software to a SensorStation V1 (though not V2 or V3). In order to do this,  follow the [CTT documentation to flash the compute module](https://cellular-tracking-technologies.github.io/ctt_documentation/flashingComputeModule.html), but use the SG software image instead of the CTT software image. You will need a WiFi adapter or Ethernet adapter plugged into your SensorStation for the initial configuration.

## Initial configuration

When the SG boots up for the first time after creating a new software SD card, it needs to be initialized by setting a password for it.&#x20;

{% stepper %}
{% step %}
### Insert the microSD card into the Raspberry Pi and power it up

Make sure you use a suitable power supply — at least 2.4 amps for Raspberry Pi 3 and 3 amps for Raspberry Pi 4.
{% endstep %}

{% step %}
### Connect to the SG's WiFi hotspot

After a minute or so, use your laptop or smartphone, look for the WiFi hotspot that the SG broadcasts. The WiFi network name will be the same as the SG serial number, e.g. `SG-B156RPI3EDBA`. If the SG has not yet been initialized, the WiFi network name will be appended with `-init` as in the example below, and will not be protected by a password.

<figure><img src="../.gitbook/assets/image (48).png" alt="" width="330"><figcaption></figcaption></figure>

This network uses a captive portal, like many airport and hotel WiFi networks, and this is how the password is set.&#x20;

<figure><img src="../.gitbook/assets/image (49).png" alt="" width="326"><figcaption></figcaption></figure>

If your browser didn't automatically launch to a "login" page or you _don't_ see any prompt or message directing you to the captive portal, you may need to visit it directly. You can access it at either of the links below.

* [**`http://192.168.7.2`**](http://192.168.7.2/)
* [**`http://sgpi.local`**](http://sgpi.local/)

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

Once at the page above, click on either of the two links to access the Initial Configuration page.
{% endstep %}

{% step %}
### Enter the password you wish to use and click `Submit`

Make sure you remember the password as this will be used for the SG's WiFi hotspot going forward _and_ the SensorGnome's web interface.

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### [Connect to your SensorGnome](connecting-to-your-sensorgnome.md) with its new WiFi hotspot

After hitting `Submit`, the initial configuration is complete. The WiFi hotspot you were connected to will disconnect and disappear, and after a minute or two, a _new_ hotspot will appear. This will have the SG serial number as its name, but will lack the `-init` suffix. This will be the primary method of [connecting to your SensorGnome](connecting-to-your-sensorgnome.md) and accessing the Web Interface going forward.&#x20;
{% endstep %}
{% endstepper %}

#### Troubleshooting







#### Alternate method of configuring during the flashing process

It's possible to configure your SG with a password during the flashing process. This feature has been deprecated in the newer Raspberry Pi Imager software, so if you wish to configure with this method, you'll need to download and use [Version 1.9.6](https://github.com/raspberrypi/rpi-imager/releases/download/v1.9.6/imager-1.9.6.exe), which is the last version to support this.

1. Plug the microSD card into your laptop/computer.
2.  Launch Raspberry Pi Imager and:

    * click on _operating system_, choose _use custom_ (it's the last option), locate the SensorGnome software ZIP you downloaded

    <br>

    <figure><img src="../.gitbook/assets/image (14).png" alt="" width="563"><figcaption></figcaption></figure>

* click on _storage_ and locate the microSD card to flash

<figure><img src="../.gitbook/assets/image (20).png" alt="" width="563"><figcaption></figcaption></figure>

*   **Important and easy to overlook**: click on the gear icon at the bottom right<br>

    <figure><img src="../.gitbook/assets/image (23).png" alt="" width="563"><figcaption></figcaption></figure>
*   Focus on the settings outlined in green

    * Set a hostname you will use to recognize your SG during the initial set-up (not used after that so use your initials, for example).
    * Set gnome as username and select a good password: it is used for all access to your SensorGnome (web UI, hotspot, remote management, SSH).
    * If you will use your local WiFi to give the SensorGnome internet access configure it here, if you will use Ethernet then skip this portion.
    * Uncheck "enable SSH" unless you have an SSH key you'd like to use, in which case you can enter it here (note that SSH is always enabled regardless of the setting here).
    * The locale setting is ignored.

    <figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>


* Save the settings and click on 'write', wait for the microSD card to be written

#### **Timing**

* you will see a few erratic blinks the first 40 seconds
* 2 flashes should start after 1 minute
* 3 flashes should start after 1:20 to 2 minutes
* it can take 3 minutes 'til the 3 flashes show up (e.g. due to lost DHCP packets), but after 2 minutes you're probably best off starting to pull out your phone and looking for the hot-spot.

#### Locating the green LED

<figure><img src="../.gitbook/assets/image (22).png" alt="" width="563"><figcaption><p>Raspberry Pi 3B/4B (here with a cellular HAT)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16).png" alt="" width="563"><figcaption><p><strong>Raspberry Pi Zero-2W</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17).png" alt="" width="563"><figcaption><p>SensorStation V1</p></figcaption></figure>
