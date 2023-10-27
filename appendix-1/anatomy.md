# Anatomy of a SensorGnome

{% hint style="warning" %}
This page has not yet been updated for the Sensorgnome V2 software!
{% endhint %}

## What's inside a SensorGnome?

Both RPi and BB SensorGnome consists of the same core components:

* Raspberry Pi or BeagleBone mini computer. This runs the software that records the raw radio pulse data. A BeagleBone SG will also include a USB hub as the BB only has one standard USB port.
* FUNcube USB dongles or other "software defined radios". These take the analog radio signals coming from the antennas and convert them into a digital format&#x20;
* GPS. This records the precise location of the SG, as well as ensures the the precise time is always set on the SensorGnome
* Associated power supply&#x20;

## Raspberry Pi SensorGnome

### Raspberry Pi

#### Ports and slots <a href="#rpi-ports-and-slots" id="rpi-ports-and-slots"></a>

The numbering of the USB ports is very important when attaching antennas since this information is recording along with detection data and can be used to determine the direction and time of approach or departure of a tagged animal.

![](../.gitbook/assets/rpiports.jpg)

The MicroSD card slot is on the opposite side as the USB and Ethernet Ports. The card is inserted with the contacts facing up, and there is no click or other indicator when the card is inserted. On some cases the the MicroSD is so deeply recessed that it cannot be removed without tweezers.

![MicroSD card is inserted with the gold contacts facing up](../.gitbook/assets/rpisdslot.jpg)

Power is supplied to a RPi through the Micro USB port. This port only supplies power and is not used to communicate with a computer.&#x20;

![Micro USB port on a Raspberry Pi](../.gitbook/assets/rpi5v.jpg)

#### Lights <a href="#rpi-lights" id="rpi-lights"></a>

LED lights can be useful in determining if the unit has power and if it is functioning properly. The RPi has fewer lights and they are less information than a BeagleBone, but they can still be helpful.

{% hint style="info" %}
There are many different cases used to house the Raspberry Pi. Not all of them permit a clear view of the lights.
{% endhint %}

The RPi itself has only two primary LED lights -- one red and one green. These are visible on the bottom right hand corner of the side that hosts the MicroSD slot. The red light indicates power, while the green light indicates CPU activity. The red light should always be on if sufficient is supplied, whereas the green light will flash sporadically

![The red light will be solid as long as adequate power is supplied; the green light will flash with CPU activity](../.gitbook/assets/rpiled1.jpg)

The attached GPS had also has an indicator light, in this case a red LED. It does not light up consistently but instead blinks occasionally. If you are having trouble connecting and the green light never illuminates, you may need a [fresh software card.](broken-reference)

![The red LED on top of the GPS hat will blink with long gaps between](../.gitbook/assets/rpigps.jpg)

Lastly, there are two indicator lights on the bottom of the Ethernet port. When the Ethernet cable is attached to a computer these lights should be on or flashing consistently.

![](../.gitbook/assets/rpiethernet.jpg)

### GPS <a href="#rpi-gps" id="rpi-gps"></a>

### Fully assembled RPi SensorGnome

![The primary components inside a typical Raspberry Pi SensorGnome](../.gitbook/assets/sginternal.jpg)

**a)** The Raspberry Pi. The colour of the RPi case may vary between SG’s but they will also be roughly the same size

**b)** FunCUBE Dongles. A Raspberry Pi SG can accommodate up to 4 dongles plugged directly into the RPi. In order to accommodate additional antennas, a USB hub would be required. The cables from the antennas will plug into the free end of the dongles.

**c)** This is the inside view of the button used to activate the WiFi hotspot.

**d)** GPS antenna. When deployed in the field, this end of the antenna would be outside the SG case, and attached to something that had a clear view of the sky. The other end is attached to the Raspberry Pi by way of the gold-coloured “SMA” port on the top right corner of this particular RPi.

**e)** Voltage converter. If powered by a solar panel and battery, as this SG is, the power coming in will be 12V. However the RPi only requires 5V, so a voltage converter is used to downgrade the current to the acceptable level. If powered directly by AC power, the wall adapter itself should output 5V, eliminating the need for a voltage converter.
