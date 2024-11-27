# Cell modem troubleshooting

## Cell modem power

Almost all cell modems compatible with Sensorgnomes use a lot of peak power and they exceed the power capabilities of rPi 3B and 4B USB ports. You _must:_

* use a cell HAT or a USB cell modem plugged into a powered USB hub, never attempt to power the cell modem from the rPi's USB ports (a cell HAT needs to be plugged into USB for data, it doesn't receive power through the cable, however)
* use a 20W power adapter (a good 15W may work)
* use short, quality USB power cables

If the cell modem does not have enough power it will seem to work fine but not connect to the network. This is because it has plenty of power while idle but experiences voltage sags when trying to transmit causing the output signal to drop or fail to modulate correctly.

### Cell HATs and USB

If you use a cell HAT it is easy to overlook that it came with a short USB cable. The 40-pin connector of the rPi provides power and some low-level controls (e.g. to detect that a cell HAT is installed). However, the cell modem is fundamentally a USB device: all the data goes over USB. Thus you must use the USB cable to connect the HAT to one of the USB ports.

### Resetting the cell modem

You can reset all the software on the Sensorgnome by rebooting, however, this has only a minor impact on the cell modem. It can help if there are errors at the USB level in communicating with the cell modem. The only way to fully reset a cell modem is to power cycle it, which in the case of a HAT means power cycling the rPi.

## SIM-o-rama

Most problems occur when starting cell service, i.e., placing a fresh SIM card into a cell modem and powering it up. When a connection does not get established quickly there may be a problem or you may simply be underestimating how slow things can be. Have enough coffee or tea handy to keep you awake, or better, plan on a couple of brief testing sessions per day over a couple of days. Or at least plan your day so you can let the modem do its thing for an hour a couple of times. Letting it noodle away over-night when it's in an enabled/registering or enabled/searching loop is not an unreasonable strategy...

### Why initiating cell service can be very slow

* Many SIMs sold for Sensorgnome type of usage work globally or continent-wide across many providers and many frequencies. This is a large space for the modem to scan and attempt to get service, either from "native" networks or via roaming.
* The cell modems used for Sensorgnomes and the software are not as sophisticated as cell phones. Things take more time.
* The "Super SIM" made by Twilio (and resold by SixFab among others) has multiple personalities used for different regions of the world. It switches personality after failing to connect for a number of minutes. You may be waiting for it to fail using the AsiaPac personality, then the China personality, then the South America personality, then...

### Try your cell phone's SIM!

If your cell phone uses a physical SIM (as opposed to an eSIM) and your Sensorgnome cell modem is in a enabled-searching-registering endless loop (i.e. doesn't manage to connect) you can try your cell phone's SIM in the Sensorgnome. You probably need one of those cut-out SIM size adapters because your phone most likely uses the nano size while the modem uses the micro size. If your phone's SIM provides service you know that the SIM or it's service is the issue (or the carriers it has access to don't cover your spot).

Note that in the US the phone SIM may not work or only work briefly if it uses the Verizon network 'cause of "approved devices" nonsense. AT\&T may have some such issues too.

### SIM activation

You did activate your SIM, right? Most SIMs used for data-only service as typically used in Sensorgnomes have some form of activation performed on some web site.

## What the Sensorgnome software does

The Sensorgnome software does not interact with the cell modem a whole lot. The `sg-control` service, which is the main data processor and also provides the web-ui, primarily queries the ModemManager every few seconds to update the state of the cell modem in the web ui.

A `check-modem` service also checks on the modem state and if it is not connected asks it to connect every few minutes. It tries to balance the need to let the modem "do its thing" trying to find a cell provider vs. ensuring that the cell modem doesn't give up and just sits there idle. Unfortunately there is no status indication for when it's still trying vs. has given up.

Once the cell modem connects everything happens automagically through a set of standard Linux services and the ModemManager. An IP address is acquired, a route to the internet is established, and its priority is such that ethernet and then wifi take precedence if they're available.

## What to look for in the Web UI

The Web UI has fairly simple info about the cellular connection. It does not allow any actions to be taken. The reason is that there isn't much that can be done without a lot of information and knowledge. Cell modems are very complex (they run a full Linux operating system) and do their thing on their own. For the Sensorgnome use-case they either work, or take time to work, or don't work and when they don't work it's very difficult to figure out where the problem lies.

### Cellular Panel

The Sensorgnome Web UI's network tab has a panel for cellular connections:

<figure><img src="../.gitbook/assets/image (21).png" alt="" width="445"><figcaption><p>Cellular panel on the network tab of the Sensorgnome Web UI</p></figcaption></figure>

This screen shot shows a modem that is successfully connected. The first item to look at is that state: the (i) brings up a brief explanation of common states. If the modem doesn't connect it often says "enabled" but then cycles through "registering" or "searching" over the course of a minute or two.

When connected ensure that the Sensorgnome got an IP address:

<figure><img src="../.gitbook/assets/image (22).png" alt="" width="296"><figcaption></figcaption></figure>

In some situations the modem may also show a list of providers at the bottom of the panel. This at least indicates that "something is working".

{% hint style="info" %}
When asking for help a screen capture of the cellular panel is very helpful! Also mention if it cycles through several states.
{% endhint %}

### APN, ip-type, and roaming

Ensure that the config box has the correct APN for your SIM provider. You should have gotten the APN when you purchased the SIM or it may be available on the SIM provider's web site.

The cell modem initially connects to a cell network and then it needs to tell the network where to route the packets so they can be billed before they enter the internet at large. This is what the APN does. For example, you may have a French Orange SIM and use it in the US. The modem may connect to AT\&T's cell network on roaming and AT\&T needs to be told to route packets to/from Orange in France. Orange will count the bytes for billing before forwarding to the internet and may apply rate limiting depending on the contract. Often, however, the SIM works even if an incorrect APN is specified because it has a default APN built-in...

The roaming flag should be on in almost all cases. It is only useful if you need to ensure the SIM doesn't roam due to cost ramifications.

The ip-type should be ipv4v6 in almost all cases, allowing both IPv4 and IPv6. There are some budget IPv4-only SIMs that _may_ need an "ipv4" setting.

## Command-line troubleshooting

Some more detailed information can be obtained via the command line through SSH. (From bitter experience, it is also easy to get lost in undecipherable LTE details and not accomplish much of anything...)

The modem is managed by the Linux ModemManager. The first step is to list the modems (there should be only one) to get the numeric ID of the one installed (typ. zero but can be other if it gets reset or the USB cable is messed with):

```
gnome@SG-7F5ERPI46977 ~> mmcli -L                                                              
    /org/freedesktop/ModemManager1/Modem/0 [Telit] LE910C4-NF                                  
```

The number of interest is after `/Modem/`. The general info and state of the modem can be queried using its id:

```
gnome@SG-7F5ERPI46977 ~> mmcli -m 0                                  
  ----------------------------------
  General  |                   path: /org/freedesktop/ModemManager1/Modem/0
           |              device id: 69b580f5cb29c45e584c21118098565c831ab3a6
  ----------------------------------
  Hardware |           manufacturer: Telit
           |                  model: LE910C4-NF
           |      firmware revision: 25.21.660  1  [Mar 04 2021 12:00:00]
           |         carrier config: default

[... about 30-40 lines ...]                                                
```

Other than info like the specific cell modem model the status section reveals whether it thinks its connected:

```
  Status   |                   lock: sim-pin2
           |         unlock retries: sim-pin (3), sim-puk (10), sim-pin2 (3), sim-puk2 (10)
           |                  state: connected
           |            power state: on
           |            access tech: lte
           |         signal quality: 92% (recent)

```

Also of interest is the operator info as well as whether a bearer is connected. The bearer "carries the internet packets" over the cell connection:

```
  Bearer   |                  paths: /org/freedesktop/ModemManager1/Bearer/1
```

The bearer can then be queried for specifics (the bearer ID increments each time the cell modem looses service and reconnects):

```
...
  ------------------------------------
  Status             |      connected: yes
                     |      suspended: no
                     |    multiplexed: no
                     |      interface: wwan0
                     |     ip timeout: 20
...
  ------------------------------------
  IPv4 configuration |         method: static
                     |        address: 100.64.0.2
                     |         prefix: 30
                     |        gateway: 100.64.0.1
                     |            dns: 8.8.8.8, 8.8.4.4
                     |            mtu: 1430
  ------------------------------------
  Statistics         |     start date: 2024-11-27T00:17:26Z
                     |       duration: 19949
...

```

To check that Linux has hooked everything up the best is to check that it has a route and then try it out:

```scss
gnome@SG-7F5ERPI46977 ~> ip route
...
default via 100.64.0.1 dev wwan0 proto dhcp src 100.64.0.2 metric 10001 mtu 1430 
...
```

This means linux thinks it can route to the internet at large (the "default") via the cellular network device (`wwan0`, or for some modems `usb0`). You can now try it out by pinging 1.1.1.1 (cloudflare dns service) using that device/interface:

```python
gnome@SG-7F5ERPI46977 ~> ping -I wwan0 1.1.1.1
PING 1.1.1.1 (1.1.1.1) from 100.64.0.2 wwan0: 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=58 time=324 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=58 time=212 ms
```

### Checking the cell phone's GPS

The GPS in most cell modems has to be explicitly activated. This is something done at boot time by a Sensorgnome script, which also finds out which USB serial device corresponds to the GPS and then allows gpsd, the GPS daemon, to listen to it.

Warning: due to a mis-feature in ModemManager it is not possible to activate the cell phone's GPS if no SIM is present in the cell modem. A cell connection is not required but a SIM is.

The ModemManager can provide info on the GPS status and activation from the modem's perspective:

```bash
gnome@SG-7F5ERPI46977 ~> mmcli -m 0 --location-status                                       
  ------------------------
  Location | capabilities: 3gpp-lac-ci, gps-raw, gps-nmea, agps-msa, agps-msb
           |      enabled: 3gpp-lac-ci, gps-nmea
           |      signals: no
  ------------------------
  GPS      | refresh rate: 30 seconds

```

The key is to see `gps-nmea` as enabled. It is unclear what "signals: no" means as in this case the GPS has a 3D fix...

In the status result of the `mmcli -m 0` command mentioned above there is a section System > ports which lists all the serial ports provided by the modem and one of them should say gps, often it's `/dev/ttyUSB2`. If all went well then `/dev/ttyGPS` should be a symlink to that serial port:

```
gnome@SG-7F5ERPI46977 ~> ls -ls /dev/ttyGPS
0 lrwxrwxrwx 1 root root 12 Nov 27 00:17 /dev/ttyGPS -> /dev/ttyUSB2
```

The best way to check further is to run the `gpsmon` utility which connects to `gpsd` to query GPS data:

```
gnome@SG-7F5ERPI46977 ~> gpsmon
tcp://localhost:2947          NMEA0183>
┌──────────────────────────────────────────────────────────────────────────────┐
│Time: 2024-11-26T23:03:52.000Z   Lat: 34 29.940000' N   Lon: 119 49.070000' W │
└───────────────────────────────── Cooked TPV ─────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────────────┐
│ GPGSV GPGGA GPGLL GPRMC GPGSA                                                │
└───────────────────────────────── Sentences ──────────────────────────────────┘
┌───────────────────────┌─────────────────────────┌────────────────────────────┐
│ SVID  PRN  Az El SN HU│Time:     230352.00      │Time:      230352.00        │
│GP  2    2  94 13 22  Y│Latitude:  3429.940000 N │Latitude:  3429.940000      │
│GP  5    5 271  7 34  Y│Longitude:11949.070000 W │Longitude: 11949.070000     │
│GP  7    7  91 45 32  Y│Speed:    0.0            │Altitude:  636.6            │
│GP  8    8  40 13 36  Y│Course:   308.2          │Quality:   1   Sats: 12     │
│GP  9    9 158 10 33  Y│Status:   A        FAA:A │HDOP:      0.5              │
│GP 13   13 312 35 36  Y│MagVar:   14.1 E         │Geoid:     -22.0            │
│GP 14   14 296 66 33  Y└───────── RMC ───────────└─────────── GGA ────────────┘
│GP 17   17 180 37 36  Y┌─────────────────────────┌────────────────────────────┐
│GP 19   19 194 11 36  Y│Mode: A3 Sats: 2 5 7 8 + │UTC:           RMS:         │
│GP 21   21  75 11 36  Y│DOP H=0.5  V=0.6  P=0.8  │MAJ:           MIN:         │
│GP 22   22 264 50 37  Y│TOFF:  0.025676591       │ORI:           LAT:         │
│GP 30   30  32 70 33  Y│PPS: N/A                 │LON:           ALT:         │
└───↓──── GSV ──────────└────── GSA + PPS ────────└─────────── GST ────────────┘
(82) {"class":"VERSION","release":"3.22","rev":"3.22","proto_major":3,"proto_min
or":14}
(293) {"class":"DEVICES","devices":[{"class":"DEVICE","path":"/dev/ttyGPS","driv
er":"NMEA0183","activated":"2024-11-26T23:03:49.023Z","flags":1,"native":0,"bps"
:9600,"parity":"N","stopbits":1,"cycle":1.00},{"class":"DEVICE","path":"/dev/pps
0","driver":"PPS","activated":"2024-11-26T23:03:10.224Z"}]}
(122) {"class":"WATCH","enable":true,"json":false,"nmea":false,"raw":2,"scaled":
false,"timing":false,"split24":false,"pps":true}
(72) $GPGSV,4,1,15,02,13,094,23,05,07,271,34,07,45,091,31,08,13,040,36,1*6F
(72) $GPGSV,4,2,15,09,10,158,33,13,35,312,36,14,66,296,33,15,05,320,21,1*68
(72) $GPGSV,4,3,15,17,37,180,36,19,11,194,36,21,11,075,36,22,50,264,37,1*6D
(52) $GPGSV,4,4,15,30,70,032,32,20,00,241,,46,,,37,1*67
(77) $GPGGA,230350.00,3429.940000,N,11949.070000,W,1,12,0.5,636.6,M,-22.0,M,,*56
...
```

This shows that the GPS has a fix and the lines with a `$GPG` prefix scroll by as the GPS reports position and time updates. If `gpsmon` just shows some cryptic info (JSON) and stops it may be worthwhile to restart `gpsd` using `sudo systemctl restart gpsd.service`&#x20;
