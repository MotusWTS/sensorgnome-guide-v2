---
description: >-
  Providing good power to Sensorgnomes and to the radios can be more challenging
  that it may seem and can result in difficult to troubleshoot issues.
---

# Sensorgnome power

When providing power to a Sensorgnome there are multiple aspects to consider:

* Powering the Raspberry Pi, which typically also powers the radios attached via USB.
* Ensuring that the Raspberry Pi can indeed power everything attached to the USB ports.
* Dealing with power when using Solar and battery systems.

## Powering USB devices

While the USB specs have a lot of rules about how much power each device may use there is a simple rule for Raspberry Pis (at least for the 3B and 4B models):

{% hint style="info" %}
All USB devices attached to and powered by an rPi 3B or 4B may draw a total of 1.2A (5V) max.

This is fine for most configurations. However, the majority of cellular modems _cannot_ be powered from the Raspberry Pi's USB ports. Use a cellular HAT instead or a powered USB hub.&#x20;
{% endhint %}

Once the power limit is exceeded the USB power is cut and devices misbehave. FUNcube dongles consume 100mA-200mA, CTT radios less than 30mA. This means that 4 FCDs and 4 CTT radios can be powered by the rPi directly. A USB GPS can also be added. The majority of cellular modems have power consumption spikes that exceed 1.2A or at least come close to it, so they cannot be powered reliably from the rPi ports. To connect a cellular modem use a HAT (printed circuit board that mounts on the rPi's 40-pin connector) or attach the dell modem to a powered USB hub.

## Powering the Raspberry Pi

The power consumption of rPis is very spiky, consuming 1A-2A at peak. All the devices attached to the USB ports and potentially a cellular HAT must be powered as well. The result is that you need to use a power adaptor that can deliver more than the typical 2.1A claimed by many power adapters. Most el-cheapo "2A" or "2.1A" adapters lead to problems sooner rather than later.

{% hint style="info" %}
rPi 3B+: use a power adaptor made for it that provides 5.1V >2.5A

rPi 4B: use a power adapter made for it or a 15W min. USB-PD (power delivery) adapter to provide 5.1V >2.5A

If using a cell modem look for 20W min.

In battery powered installs ensure you use a 20W min. voltage regulator / converter (to regulate down from the approx 12V of a typ. battery to 5V) and if the voltage is adjustable (typ. it's not) then tune it up to 5.1V-5.2V.

I all cases use a quality cable that is as short as practical: cheap cables with undersized conductors and connectors with poor strain relief end up costing a fortune in headaches and site visits.

Check for "undervoltage condition" in the system log (`/var/log/syslog`)
{% endhint %}

When a Sensorgnome isn't receiving enough power -- that is, the voltage and/or current is lower than the device is rated for -- it can result in a malfunctioning station that doesn't collect data.&#x20;

### Using Power over Ethernet (PoE)

While Raspberry Pis don't have power-over-ethernet (PoE) built-in it is possible to use PoE with "splitters" that separate the power and ethernet at the device end. For reference, a good page about PoE is https://en.wikipedia.org/wiki/Power\_over\_Ethernet and it's ... confusing... The table at the end of the page in the Pinouts section is a good summary.

The recommended set-up with PoE is to use a PoE switch (which will most likely use "gigabit mode A" in the table) and a splitter with a built-in voltage converter. The splitter to use depends on the rPi model: for an rPi3 get a splitter that provides 5V on a micro-USB connector (example [https://www.amazon.com/gp/product/B07CNKX14C](https://www.amazon.com/gp/product/B07CNKX14C/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8\&psc=1)), for an rPi4 get a USB-C splitter (example [https://www.amazon.com/UCTRONICS-PoE-Splitter-USB-C-Compliant/dp/B087F4QCTR](https://www.amazon.com/UCTRONICS-PoE-Splitter-USB-C-Compliant/dp/B087F4QCTR/ref=sr_1_2_sspa?crid=1SF3TJCS9Z02C\&dib=eyJ2IjoiMSJ9.rGxZdSEeMTnPz2-Sk_U4Yuhnq74ngpc0onM6N3DBQAd-FRXUSVogjqrAfqfe07TM915FNPFBNt4h_L5-YPV5Ce2UdKcMnX9xBF817Z0chMC9uF0SgaOcYhG58cp1w_FTHS06GFBGQFDHe1mf96gPkT9HG4uvKvlozzzW416KoLa7fX_jxIQrKLXgzJVhznjcjfLkx3gG1X5SXsTm5i5YHMHsdADqPu2iOgSmHy8kM44.xst_NNLDDx65EZ58ZGW63g0CX72TiRtYyoYOv3FUN6I\&dib_tag=se\&keywords=poe+splitter+usb-c+pd\&qid=1733157954\&sprefix=poe+splitter+usb-c+pd%2Caps%2C254\&sr=8-2-spons\&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY\&psc=1)), for a SensorStation get a splitter that provides 12V (example [https://www.amazon.com/gp/product/B0CL2CBS75/](https://www.amazon.com/gp/product/B0CL2CBS75/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8\&psc=1)). For a rPi4 it may be advantageous to use a splitter that supports PD (power delivery) so it outputs 5.1V, but those are more pricey and possibly overkill. The above links are intended as suggestions not recommendations as the longevity of these devices has not been tested, there are definitely more "industrial" versions available.

Overall, the important part is to ensure that the switch/injector and splitter match. Virtually all PoE switches in 2024 are Gigabit 802.3af (48V) compliant (or more advanced 803.2at/bt) and use "mode A" (see Wikipedia table). Using a "gigabit" and/or "802.3af" splitter ensures compatibility assuming it's properly made.&#x20;

A common alternative that historically predates 802.3af is to use 2 wire pairs for 100Mbit ethernet and 2 pairs for 24V. This is still common in Ubiquiti equipment as well as that of other vendors, which perpetuates the confusion. In addition, this mode of operation is "covered" by 802.3af mode B, except that 802.3af requires 48V, not 24V. But 802.3af splitters as mentioned above _should_ work fine with 24V too.

One catch with non-gigabit splitters (e.g. the 24V set-up) is that if they are used to connect two gigabit devices (switch and rPi) then it is often necessary to force the use of 100Mbps at one of the two ends, i.e. either using the switch web interface (in the case of a managed switch) or `ethtool` on the rPi.

### Underpowered Sensorgnome - how does it occur?

Underpowered Sensorgnomes typically work fine when set-up and it's later that problem crop up. Sometimes they reboot but more often some peripheral malfunctions. Often USB-attached devices restart on their own. In some situations the Sensorgnome just seems "flaky" and a different problem crops up every time.

The easy cases to troubleshoot are due to inadequate components and installation problems because they tend to show up quickly. Check components, have spares to swap-in to verify against, check all connections and connectors.

The troublesome cases are when general wear and tear triggers the problem. This is because USB cables and power cables experience a fair bit of strain over their lifetime from repeated bending. The following problems have been identified at Motus stations in the past:&#x20;

* The connection to the screw terminals on the DC-DC voltage converter is loose
* Individual copper strands within the USB cable are broken. This can occur from repeated bending of the cable.
* The microUSB port of the Raspberry Pi is damaged (lifts from the circuit board slightly when force is applied).
* Due to nightly deep discharges the off-grid battery can no longer reliably power the Sensorgnome through the night.
* Due to humidity and heat-cycling connectors become unreliable.

### Identifying an underpowered Sensorgnome

The behaviour of underpowered devices can be inconsistent and hard to diagnose. In some cases, it is not possible to connect to the device because both Ethernet and Wi-Fi are malfunctioning, however it may still appear as though the device is on as the indicator LEDs will be blinking. This can also be due to corrupted data on the SD card or a physical connection problem with the SD card, so it's not always obvious.&#x20;

{% hint style="info" %}
In our experience, on remote field trips it's always best to have a complete spare Sensorgnome available so that components can be swapped out and tested.&#x20;
{% endhint %}



{% hint style="info" %}
While it might seem like it's helpful to use a voltmeter to determine whether the power supply is the issue, it's often that the USB cable is the culprit which hard to measure. Also, the power consumption has significant spikes that last only a few milliseconds and a voltmeter does not show resulting power sag issues.
{% endhint %}

### Fixing power supply issues

{% hint style="info" %}
This section has not been updated in a number of years. In short:

* First focus on cables and replace cables, focus on quality and on not bending too tightly. Possibly rearrange components in the enclosure to avoid squishing cables into tight bends.
* Then check power adapters, perhaps it's time to use a (specially made) 5.1V adapter for an rPi 3B or to ensure you have at least a 20W USB-PD (power delivery) adapter for an rPi 4B (without cell modem 15W should be enough if the adapter can actually deliver them).
* For off-grid installs there is the added complication of ensuring the battery can still deliver power peaks, especially if in service for over a year. Also, there are more ad-hoc cables and connectors in play offering more points of subtle failure.

In terms of sources:

* Shops that sell Raspberry Pis have power adapters, including original Raspberry Pi ones.
* Anker cables and adapters are generally of decent quality, found on Amazon among other sources.
* Digikey has fancy cables and a huge selection of power adapters (DC-DC converters).
{% endhint %}



{% hint style="warning" %}
Always keep a spare micro USB cable handy, one that has at least 22 gauge power delivery. See [USB Micro B Male cables for power delivery](sensorgnome-power.md#usb-micro-b-male-cables-for-power-delivery).&#x20;
{% endhint %}

In most cases, fixing the issue is as simple as replacing the Micro USB cable which plugs into the Raspberry Pi. It is also possible to swap out the DC-DC voltage converter to a device that is rated for more current and/or voltage (no more than 5.1 V, however!), but these have not been tested. See: [DC-DC Voltage Converters for Raspberry Pi (not tested)](sensorgnome-power.md#dc-dc-voltage-converters-for-raspberry-pi-not-tested).

### USB Micro B Male cables for power delivery

Currently available (as of Nov 3, 2021) cables are listed below:

<table><thead><tr><th>Retailer</th><th>Price</th><th width="208">Gauge</th><th>Link</th></tr></thead><tbody><tr><td>DigiKey</td><td>$6.21 CAD</td><td>20 AWG</td><td><a href="https://www.digikey.ca/en/products/detail/tripp-lite/UR05C-003-UARB/5359414">Product page</a></td></tr><tr><td>MonoPrice</td><td>$0.99 USD (0.5 ft)</td><td>22 AWG</td><td><a href="https://www.monoprice.com/product?p_id=13924">Product page</a></td></tr></tbody></table>

### DC-DC Voltage Converters for Raspberry Pi (not tested)

Currently available (as of Nov 3, 2021) DC-DC converters are listed below. Please note that these models have not been tested, but are expected to perform better the default model.

<table><thead><tr><th width="150">Retailer</th><th width="150">Price</th><th width="150">Volts (V)</th><th width="162">Watts (W)</th><th>Link</th></tr></thead><tbody><tr><td>DigiKey</td><td>$60 USD</td><td>5.1 V</td><td>20 W</td><td><a href="https://www.digikey.ca/en/products/detail/xp-power/DTE2024S5V1/5931159">Product page</a></td></tr><tr><td>DigiKey</td><td>$38.5 USD</td><td>5.0 V</td><td>20 W</td><td><a href="https://www.mouser.com/ProductDetail/490-PYB15-Q24-S5-T">Product page</a></td></tr><tr><td>DigiKey</td><td>$51 USD</td><td>5.1 V</td><td>10 W</td><td><a href="https://www.digikey.ca/en/products/detail/traco-power/TMDC-10-2411/9698249">Product page</a></td></tr></tbody></table>
