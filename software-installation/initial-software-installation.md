---
description: Flashing a microSD card, booting the Sensorgnome, and connecting to it.
---

# Initial software installation

The initial installation consists of:

1. flashing a microSD card with a Sensorgnome release image,
2. booting the rPi and connecting to it.

## Prerequisites

* MicroSD-card with at least 16 GB and an adapter/reader to plug into your computer.
* A computer with Windows/Mac OS/Linux.
* The latest **Sensorgnome software** release downloaded from [https://www.sensorgnome.net/download](https://www.sensorgnome.net/download)
* **Raspberry Pi Imager** installed on your computer, it can be downloaded from [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/) (alternate SD card flashing software can be used as well, such as WinDiskImager, Batena Etcher, and others).
* **RaspberryPi** 4B, 3B+, 3B, Zero-2W, or SensorStation V1.
* Highly recommended: **internet access** for the RaspberryPi either via WiFi or ethernet.

## Multiple paths to success

There are multiple options for installing and configuring the software. The steps outlined here follow the recommended route. If something is not working for you: try an alternate route.

In any case: **don't spend more than 10 minutes being blocked!** If it just doesn't work and you double-checked your steps: [ask for help](../#getting-help).

## Flashing the SD-card

{% hint style="info" %}
Please use a quality microSD-card! The Samsung Pro Endurance series as well as the SanDisk Extreme Pro series are highly recommended (and under $10!). Please buy where you can ensure you are getting genuine cards, e.g. on Amazon check that the cards are "sold by Amazon.com".
{% endhint %}

{% hint style="info" %}
SensorStations: To flash a SensorStation follow the CTT documentation to prepare your computer for the flashing operation. Use the RaspberryPi Imager as described here, however. You will need a Wifi adapter or Ethernet adapter plugged into your SensorStation.
{% endhint %}

### Steps

1. Plug the microSD-card into your laptop/computer.
2. Launch RaspberryPi Imager and:
   * click on _operating system_, choose _use custom_ (it's the last option), locate the Sensorgnome software ZIP you downloaded\
     <img src="../.gitbook/assets/image (2) (1).png" alt="" data-size="original">![](<../.gitbook/assets/image (8).png>)
   * click on _storage_ and locate the microSDcard to flash![](<../.gitbook/assets/image (14).png>)
   * **Important and easy to overlook**: click on the gear icon at the bottom right\
     ![](<../.gitbook/assets/image (17).png>)
   * Focus on the settings outlined in green\
     ![](<../.gitbook/assets/image (1) (1) (1).png>)
     * Set a hostname you will use to recognize your SG during the initial set-up (not used after that so use your initials, for example).
     * Set gnome as username and select a good password: it is used for all access to your Sensorgnome (web UI, hotspot, remote management, SSH).
     * If you will use your local WiFi to give the Sensorgnome internet access configure it here, if you will use Ethernet then skip this portion.
     * Uncheck "enable SSH" unless you have an SSH key you'd like to use, in which case you can enter it here (note that SSH is always enabled regardless of the setting here).
     * The locale setting is ignored.
   * Save the settings and click on 'write', wait for the microSD-card to be written

{% hint style="info" %}
Not using RaspberryPi Imager?\
If you are using a program other than the RaspberryPi Imager to flash the microSD card then you cannot enter the initial configuration of hostname, password, and Wifi as shown above. You have two options: use the hotspot access method described below or configure internet access after flashing by placing a `wpa_supplicant.conf` file into the boot partition. The latter is a standard RaspberryPi procedure so you can follow the RaspberryPi docs.
{% endhint %}

## Booting your Sensorgnome

During the boot process the RaspberryPi green LED provides feedback. Proceed as follows:

1. Insert the microSD card into the RaspberryPi and apply power
2. The red LED on the RaspberryPi indicates power
3. The green LED comes on solid when the Pi boots, thereafter it blinks about every 2 seconds:
   * 1 flash: operating, hotspot off, and no internet access
   * 2 flashes: operating, hotspot ON, no internet access
   * 3 flashes: operating, internet access OK (hotspot may be on or off)

Once you see the 2 or 3 flashes pattern you can proceed.

#### **Timing**

* you will see a few erratic blinks the first 40 seconds
* 2 flashes should start after 1 minute
* 3 flashes should start after 1:20 to 2 minutes
* it can take 3 minutes 'til the 3 flashes show up (e.g. due to lost DHCP packets), but after 2 minutes you're probably best off starting to pull out your phone and looking for the hot-spot...

#### Locating the green LED

Raspberry Pi 3B/4B (here with a cellular HAT):![](<../.gitbook/assets/image (16).png>)

Raspberry Pi Zero-2W:\
![](<../.gitbook/assets/image (10).png>)

SensorStation:\
![](<../.gitbook/assets/image (11).png>)

#### Troubleshooting

* If you never get to a blinking green LED try the following:
  * use a different power supply
  * unplug any attached USB devices
  * reflash the microSD card,
  * use a different microSD card.
* If you only see 2 flashes but expected the RaspberryPi to have internet access, try the following options:
  * wait another minute
  * proceed using the hotspot
  * ensure you used the correct SSID and password in the RaspberryPi Imager configuration (i.e., reflash the card)
  * use your phone to check that the WiFi works and provides internet access

## Choosing how to connect to your Sensorgnome

You have two options for connecting to your Sensorgnome: hotspot and internet. Both have advantages and disadvantages. One works better for some users, others are more successful with the other... Pros and cons:

#### WiFi Hot-Spot

* pro: local, no internet required
* pro: quicker to be ready than internet
* con: connecting can be tricky and confusing depending on laptop/phone, for some users it just works for others it never does

#### Internet

* pro: if your RaspberryPi shows 3 flashes it's almost guaranteed to work
* con: requires internet access for both RaspberryPi and laptop/phone
* con: currently requires that both the RaspberryPi and laptop/phone are connected to the same local network (i.e., that the RaspberryPi can be accessed directly by the laptop/phone, this doesn't work, for example, if the phone is on cellular and the RaspberryPi on local WiFi)

{% hint style="info" %}
If you configured your Sensorgnome with internet access you have both options. If one gives you trouble: try the other!!!\
Also, if you run into issues and the RaspberryPi has internet access then it will upload log files and you can [get help](../#getting-help).
{% endhint %}

## Connect to your Sensorgnome via Internet

First: verify that your RaspberryPi's green LED shows 3 flashes every 2 seconds.

* If it doesn't your RaspberryPi does not have internet access: use the hotspot method or double-check the internet access (e.g. if using WiFi reflash and triple-check the WiFi settings).

Open the Sensorgnome Hub (SGhub) website at [https://www.sensorgnome.net](https://www.sensorgnome.net) (provisionally, the username and magic word are motus and manygnomes, respectively).

The table at the top shows the most recently initialized Sensorgnomes: yours may already be at the top! You can identify it using the label column which should show the hostname you entered.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Click on the row with your Sensorgnome, this switches to the station tab; at the top-right is a link to the web UI:\


<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

You should see a set-up page for your Sensorgnome to confirm the password, this is needed to set the hot-spot password:\
![](<../.gitbook/assets/image (12).png>)

After verifying the password the web UI should load, but it may take 20-30 seconds and show "odd transitional" views, please have patience and/or reload your browser tab. (There is room for improvement here!)\
In the end, you should see the login box into which you have to enter the password you entered into the RaspberryPi Imager:\
![](<../.gitbook/assets/image (4) (1).png>)

You are now connected to the web UI:\
![](<../.gitbook/assets/image (5).png>)

## Connect to your Sensorgnome via its WiFi hot-spot

{% hint style="info" %}
The hot-spot acts as a "captive portal", which means that it looks to your computer/phone like a WiFi network typical of airports or hotels where you have to enter credentials or accept an agreement to get internet access. However, the Sensorgnome never actually provides internet access...

Some of the issues users have encountered are:

* a web browser automatically opens to show the Sensorgnome's web UI, but it may be a special system browser and not your regular browser (on iOS that browser seems not to show certain charts)
* your computer/phone may become "impatient" about not getting internet access and decide to switch back to your regular WiFi
* if using Android, your phone may use the cellular network instead of the Hot-Spot and you most likely have to turn cellular/mobile off

In many cases the hot-spot "just works" but being aware of the potential issues helps.

If you get stuck and "it doesn't work" check that your computer/phone is still connected to the hot-spot. You can also connect to the Sensorgnome using your favorite browser at `http://192.168.7.2` to avoid the system browser.
{% endhint %}

1. Open the WiFi/internet control panel on your computer/phone and connect to a network named something like `SG-1234RPI4ABCD-init` that is _not_ secure/encrypted, i.e. no password.
2. After connecting, a browser should pop-up showing a Sensorgnome page:
   * if you _did not_ use the RaspberryPi Imager and set a password there, you will be asked to configure a password
   * if you _did_ use the RaspberryPi Imager and set a password you will be asked to confirm the password\
     **Note**: this is currently broken: you will just be redirected to the web UI login and you will be disconnected from the hot-spot 10 seconds after you log in and you will have to reconnect
3. After submitting the password, you will see a confirmation page after which the hot-spot temporarily turns off and you will thus be disconnected.
4. You now need to reconnect to the "real" hot-spot which has the same `SG-1234RPI4ABCD` type of id but without trailing -init. This hot-spot is now secured with the password, so you will need to enter it (again...).
5. A browser should again pop-up and display a simple redirect page that leads you to the web UI's login page.

In the future you will be able to get to the web UI via the hot-spot just using steps 4 & 5. The prior steps are only for the first time to set the hot-spot password.

## Next steps

You now have access to the Sensorgnome web UI. Proceed with the next section to set-up the radios and network connections.

## FAQ

### **How does the hot-spot captive portal work?**

When a device connects to the hot-spot it detects that there is no internet connection (because the hot-spot only provides access to the Sensorgnome). It then assumes that this is a "captive portal", which means that the user has to connect to a specific web site to log in or agree to some legal terms before getting internet access. This is typical of wifi in public locations, e.g. airport, hotel, or coffee shop.

In order to facilitate the required login or acceptance of terms your device starts a web browser which connects to the Sensorgnome's web UI. This usually works well and allows you to use the UI. However, depending on the device and on other available wifi networks you may run into issues. The main cause for issues is that your device expects to eventually get internet access but the Sensorgnome will never provide that, so your device may take actions in an effort to restore internet access. Specifically, your device may decide to disconnect from the Sensorgnome hot-spot and connect to some other network, such as a previously working one.

Tips:

* If your device prompts you with a message stating that this network does not provide internet access and whether you want to stay connected anyway choose the option to stay connected.
* If the web UI stops working, check whether your device disconnected from the hot-spot and connected to a different network. If so, reconnect to the hot-spot.
* The web browser used in the captive portal mode (i.e. to allow you to log-in or accept terms) may not be the regular web browser you use on your device. If it does not work well or is closed on you open your standard browser and try `http://192.168.7.2` or `http://sgpi.local`.
* If the captive portal ends up being broken or too confusing turn it off on the landing page at `http://192.168.7.2`, `http://sgpi.local`. Then possibly disconnect and reconnect to the hot-spot and navigate to one of the those two URLs explicitly by bringing up a browser. Your device operating system will warn about "no interenet access" and some "stay connected anyway" setting may be necessary.

### **Why a disk image?**

The reason the Sensorgnome release is distributed as disk images is that the system requires 2 disk partitions (two filesystems). One small FAT32 partition which the boot loader understands and can load the initial program from. And then one large partition that has a proper Linux filesystem that is way too complex for a bootloader and that the entire system runs from (and that "gets started" by that "initial program"). The image packs everything together (and is the standard way of doing these things).

Previous versions of Sensorgnome had only one large FAT32 partition occupying the entire SDcard and instead of having a second partition for the linux filesystem they put that inside one big file within the FAT32 partition. So it was a bit like nested dolls. Technically that works, but the performance suffers because every filesystem access requires two levels of mapping and access, and it's very unconventional though creative.

### **Why does flashing require root/admin/superuser permissions?**

In order to write an image to a disk one has to read/write to the raw disk, which inherently provides access to all the data that may be on the disk. That's a security issue in that it circumvents all the access controls that the operating system normally imposes on disk access. For this reason the operating system only allows the super-user/root/admin to access raw disk. And that's why Etcher (and any program that writes an image) has to ask for this permission.

### **Does the hot-spot turn on at boot time?**

Currently the hot-spot always turns on at boot time. It can be turned on/off on the network tab of the web UI. It is planned to provide a switch to enable/disable it at boot, but for the moment to facilitate debugging the hot-spot is always on at boot.

### **What are hostnames of the form 192-168-0-18.my.local-ip.co for?**

The local-ip.co host names have to do with HTTPS. A host name of the form `A-B-C-D.my.local-ip.co` resolves to the IP address `A.B.C.D`, this is how the connection is routed to the Sensorgnome. The Sensorgnome's web server holds a wildcard TLS certificate for `*.my.local-ip.co` which allows the user's web server to connect without warning or issues.

The wildcard certificate is issued by Lets Encrypt and has a validity duration of 3 months. This means that the certificate needs to be updated by some means, this is not currently implemented.

### **Why doesn't the Sensorgnome use a self-signed certificate?**

Browser support for self-signed certificates has been steadily shrinking. The warnings have been getting more dire and some browsers on some operating systems have eliminated support entirely. Initial testing with self-signed certificates resulted in lots of difficulties and confusion.

### **What are all the partitions on the SDcard for?**

When the SDcard is initially flashed from the image there are two partitions/filesystems:

* a 256MB `boot` partition holding a FAT32 filesystem that is used in the initial boot stage.
* an approx 4GB `rootfs` partition holding an EXT4 Linux filesystem with the operating system, this partition cannot (easily) be mounted on a Windows system and may show up as empty or unused, but it certainly isn't!
* the rest of the SDcard is empty/unpartitioned.

When the SDcard is first booted a third partition is created:

* a large `data` partition filling the rest of the SDcard (e.g. about 26GB on a 32GB card) holding a FAT32 filesystem that is used to store the Sensorgnome's config and data.

### **What time is reported for detections that occur before time synchronization?**

The Raspberry Pis do not have a real-time clock (RTC) chip that retains the time between system reboots or power cycles. Time is only kept while the operating system is running. The operating system writes the current time to a file every 11 minutes. At boot time, time is initialized to the last thus saved timestamp. As a result, until a fresh time synchronization happens (whether Network Time Protocol NTP or GPS) the Sensorgnome thinks it did a quick reboot right after saving the time.

There are three ways the Sensorgnome can synchronize time: NTP, GPS, or RTC chip. If the SG has internet connection it will start trying to synchronize via NTP. If the SG has a GPS (such as the Adafruit GPS HAT) it will synchronize as soon as the GPS signals a fix. If the SG has an Adafruit GPS HAT and the RTC built into that HAT's GPS module has the time (i.e. power was not lost or the battery is functioning) then the RTC time will be used.

Radio tag/pulse detections occur independently of time synchronization, i.e., the SG does not wait for time sync before starting the radios. For any detections that are made the name of the file in which the detections are save has a letter that indicates the time sync state. Specifically, if the letter at he end of the ISO timestamp in the filename is the std 'Z' then the time is synchronized, if the letter is a 'P' then it is not.

Note that in older versions of the software the timestamps of a Sensorgnome without time syn were way in the past, such as pre-2010. This is no longer the case because it prevents HTTPS communication due to the fact that all certificates are flagged as invalid.

### How do I connect a generic/discrete GPS?

_This section is a little outdated. Please use the Adafruit HAT or a cellular modem (which has a GPS in it). Alternatively, connect a GPS via USB such as the module mentioned at the end below._

If you are not using the Adafruit GPS HAT or the Sixfab cellular HAT you can connect a "generic" GPS breakout board. Please purchase from a reputable seller as many "U-Blox" GPS breakouts are fake and contain knock-off devices. A (genuine) U-Blox GPS is a good choice because it supports a fast binary protocol which gives good time synchronization without PPS (pulse per second) signal.

You can connect a GPS either via USB or serial. If you use USB "it should just work", although if the web UI shows "no-dev" 5 minutes after boot then contact the sensorgnomads mailing list or see below. (It does sometimes take a couple of _minutes_ for gpsd to detect and configure the GPS device and then the Web UI to figure that out, use `gpsmon` on the commandline to get quicker feedback.)

You can also connect via serial using 4 jumper wires. You will need to connect to the Raspberry Pi's GND, 5V, Uart-TX/gpio14, and Uart-RX/gpio15. Search for "raspberry pinout" and you will find many pictures that show the rPi connector pin assignment. The 4 pins you need to connect to are all next to each other on the outer row. Note that TX on the rPi goes to RX on the GPS module and RX on the rPi to TX on the GPS.

After hooking up the GPS hardware you need to boot your rPi and SSH in. Then issue the command:\
`sudo raspi-config nonint do_serial 2`\
and reboot. This disables the linux login console on the serial port but keeps the port enabled. If you prefer to use raspi-config interactively (i.e. run `sudo raspi-config`) what you need to do is select the primary serial port, disable the console on it, but keep the port enabled.

To troubleshoot the GPS log in via SSH and run `gpsmon`, if you just get a couple of lines then gpsd (the GPS management daemon) is not talking to the GPS. Double-check your connections (especially RX-TX cross-over), ensure the Sensorgnome is connected to the internet so it can upload its log files and contact the sensorgnomads mailing list with the ID of your Sensorgnome. If gpsmon shows lots of GPS info that updates every second or two then your GPS is working fine.

Another troubleshooting avenue is `/var/log/syslog`: restart gpsd (`sudo systemctl restart gpsd`) and look at what it prints in `/var/log/syslog`. It goes through a number of devices, including `/dev/ttyUSB0` and `/dev/serial0`. If you see an error for the device your GPS is using that may provide clues about what is going wrong.

For USB-connected GPS modules the tty device created may not be in the list scanned by GPSD. Use `lsusb` to verify the presence of your GPS device and `ls -l /dev/serial/by-id` to glean which ttyXXX port is used. Then edit `/etc/default/gpsd` to make sure it's in the list and restart gpsd (`sudo systemctl restart gpsd`).

In the US, the author has been successful with 2 purchases of the following GPS module available on Amazon: https://www.amazon.com/gp/product/B07P8YMVNT and the serial connection looks something like this (please use pinout diagrams!):

<figure><img src="../.gitbook/assets/PXL_20221030_173643526.jpg" alt=""><figcaption><p>GPS connected to rPi3 using serial cable for illustration (this is not a permanent install...)</p></figcaption></figure>

\
\
\
