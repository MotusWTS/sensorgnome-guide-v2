# Additional information

### **Why a disk image for the software?**

The reason the SensorGnome release is distributed as disk images is that the system requires 2 disk partitions (two filesystems). One small FAT32 partition which the boot loader understands and can load the initial program from. And then one large partition that has a proper Linux filesystem that is way too complex for a bootloader and that the entire system runs from (and that "gets started" by that "initial program"). The image packs everything together (and is the standard way of doing these things).

Previous versions of SensorGnome had only one large FAT32 partition occupying the entire SDcard and instead of having a second partition for the linux filesystem they put that inside one big file within the FAT32 partition. So it was a bit like nested dolls. Technically that works, but the performance suffers because every filesystem access requires two levels of mapping and access, and it's very unconventional though creative.

### **Why does flashing require root/admin/superuser permissions?**

In order to write an image to a disk one has to read/write to the raw disk, which inherently provides access to all the data that may be on the disk. That's a security issue in that it circumvents all the access controls that the operating system normally imposes on disk access. For this reason the operating system only allows the super-user/root/admin to access raw disk. And that's why Etcher (and any program that writes an image) has to ask for this permission.

### **Does the hot-spot turn on at boot time?**

Currently the hot-spot always turns on at boot time. It can be turned on/off on the network tab of the web UI. It is planned to provide a switch to enable/disable it at boot, but for the moment to facilitate debugging the hot-spot is always on at boot.

### **What are hostnames of the form 192-168-0-18.my.local-ip.co for?**

The local-ip.co host names have to do with HTTPS. A host name of the form `A-B-C-D.my.local-ip.co` resolves to the IP address `A.B.C.D`, this is how the connection is routed to the SensorGnome. The SensorGnome's web server holds a wildcard TLS certificate for `*.my.local-ip.co` which allows the user's web server to connect without warning or issues.

The wildcard certificate is issued by Lets Encrypt and has a validity duration of 3 months. This means that the certificate needs to be updated by some means, this is not currently implemented.

### **Why doesn't the SensorGnome use a self-signed certificate?**

Browser support for self-signed certificates has been steadily shrinking. The warnings have been getting more dire and some browsers on some operating systems have eliminated support entirely. Initial testing with self-signed certificates resulted in lots of difficulties and confusion.

### **What are all the partitions on the SDcard for?**

When the SDcard is initially flashed from the image there are two partitions/filesystems:

* a 256MB `boot` partition holding a FAT32 filesystem that is used in the initial boot stage.
* an approx 4GB `rootfs` partition holding an EXT4 Linux filesystem with the operating system, this partition cannot (easily) be mounted on a Windows system and may show up as empty or unused, but it certainly isn't!
* the rest of the SDcard is empty/unpartitioned.

When the SDcard is first booted a third partition is created:

* a large `data` partition filling the rest of the SDcard (e.g. about 26GB on a 32GB card) holding a FAT32 filesystem that is used to store the SensorGnome's config and data.

### **What time is reported for detections that occur before time synchronization?**

The Raspberry Pis do not have a real-time clock (RTC) chip that retains the time between system reboots or power cycles. Time is only kept while the operating system is running. The operating system writes the current time to a file every 11 minutes. At boot time, time is initialized to the last thus saved timestamp. As a result, until a fresh time synchronization happens (whether Network Time Protocol NTP or GPS) the SensorGnome thinks it did a quick reboot right after saving the time.

There are three ways the SensorGnome can synchronize time: NTP, GPS, or RTC chip. If the SG has internet connection it will start trying to synchronize via NTP. If the SG has a GPS (such as the Adafruit GPS HAT) it will synchronize as soon as the GPS signals a fix. If the SG has an Adafruit GPS HAT and the RTC built into that HAT's GPS module has the time (i.e. power was not lost or the battery is functioning) then the RTC time will be used.

Radio tag/pulse detections occur independently of time synchronization, i.e., the SG does not wait for time sync before starting the radios. For any detections that are made the name of the file in which the detections are save has a letter that indicates the time sync state. Specifically, if the letter at he end of the ISO timestamp in the filename is the std 'Z' then the time is synchronized, if the letter is a 'P' then it is not.

Note that in older versions of the software the timestamps of a SensorGnome without time syn were way in the past, such as pre-2010. This is no longer the case because it prevents HTTPS communication due to the fact that all certificates are flagged as invalid.
