# CTT Motus dongles



In order to detect CTT 434 MHz tags on a SensorGnome, you will need the appropriate dongle as FunCubes will not work for these tags. These are often referred to as "CTT Motus dongles."

{% hint style="info" %}
_CTT stopped manufacturing these dongles in early 2023. However, they have generously provided the firmware and 3D printing schematics that make it possible to build and assemble your own._&#x20;
{% endhint %}

**1)** Acquire the necessary components and files

* [**AdaFruit Feather 32u4 Radio (RFM69HCW)**](https://www.adafruit.com/product/3077)
* [**uFL surface mount antenna connector**](https://www.adafruit.com/product/1661)
* (2-3) [M1-0.25 x 2mm screws](https://www.amazon.com/uxcell-Stainless-Phillips-Furniture-Connection/dp/B0C1SCVNDZ?th=1) (or similar), for securing Feather to case
* [**uFL to SMA bulkhead adapter**](https://www.amazon.com/gp/product/B07P71FX45/ref=ox_sc_act_title_1?smid=A1TE63QTMAEOQO\&th=1) (a shorter, more flexible cable is preferred)
* [**USB-A to microUSB cable**](https://www.amazon.com/SABRENT-Charging-Android-Premium-Charger/dp/B011KMSNXM?th=1) **or** [**USB-C to microUSB cable**](https://www.adafruit.com/product/3879)**,** depending on your computer's USB port availability. If you can use a USB-A cable with your computer, and get a short one similar to the example linked, you can use it later when installing the CTT Dongle as part of the SensorGnome.
* **Arduino Feather Programmer and CTT Dongle firmware bundle below.&#x20;**_**NOTE:** The firmware currently included in the below bundle is the last commercially-used firmware on the CTT dongle. Thus it does NOT require the jumper wire described and referenced elsewhere in Motus mailing list to function properly. If the firmware provided here is updated to the newest release requiring the jumper wire, these instructions will be modified to reflect that change._

{% file src="../.gitbook/assets/CTT_dongle_Feather_programmer_w_firmware.zip" %}

{% file src="../.gitbook/assets/CTT_dongle_case_STL_files.zip" %}

**2)** Connect the AdaFruit Feather board to a USB port on your computer. The Feather board has a microUSB connection, so you'll need the USB cable.

**3)** Unzip the Arduino Feather Programmer and CTT Dongle firmware bundle to your computer. You will have to extract or copy the entire `CTT_dongle_Feather_programmer_w_firmware` folder contained in the zip file to a new, physical location on your computer. You cannot run the firmware installation program from within the zipped file.

**4)** Within the newly unzipped directory, run the `Feather32u4Programmer.exe` program. It should look something like this:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>CTT Dongle programming window</p></figcaption></figure>

**5)** Once the program window opens, from the drop-down box, select the COM port that the Feather device is connected to. If you have multiple COM ports listed, you can identify the correct port by (a) noting which COM ports are listed, (b) unplugging the device, (c) pressing the`Refresh` button, (d) see which port has disappeared from the list of COM ports, (e) plugging the device back in, and (f) refreshing again and selecting the new COM port.

**6)** Press the `Select File` button and navigate to the `station_radio_last_commercial.hex` file located in the `Firmware` directory of the bundle you unzipped in Step 2.

**7)** Press the `Program` button at the bottom of the window and wait until the program indicates the firmware installation is done. You may then close the program and disconnect the Feather device.

**8)** Solder the uFL surface mount connector to the underside of the Feather ([see here for tips](https://learn.adafruit.com/adafruit-feather-32u4-radio-with-rfm69hcw-module/antenna-options#ufl-antenna-2432562)).

**9)** Print the body and lid of the CTT dongle case with your 3D printer.

**10)** Install the SMA bulkhead first, tightening it securely and helping the bulkhead to fit into the hexagonal opening that fits its base.

<figure><img src="../.gitbook/assets/dongle_case.jpg" alt=""><figcaption><p>SMA bulkhead installed and tightened securely.</p></figcaption></figure>

**11)** Attach the uFL connector to the newly-installed uFL surface mount connector on the bottom of the Feather. This connection can easily come undone, so secure with some liquid electrical tape or high temperature hot glue.

<figure><img src="../.gitbook/assets/uFL_liquid_electrical_tape.jpg" alt=""><figcaption><p>uFL connector secured with liquid electrical tape.</p></figcaption></figure>

**12)** Insert the Feather into the case, carefully maneuvering the SMA to uFL cable under the board and around the 3D printed board support, aligning the corner mounting holes with the screw mounts in the bottom of the case. Install 2-3 of the M1 screws in the corner, securing the board to the base.

<figure><img src="../.gitbook/assets/dongle_screws_installed.jpg" alt=""><figcaption><p>M1 screws in and secure. Install at least one on each end of the Feather board.</p></figcaption></figure>

**13)** Attach the case lid. It may be easiest to insert one long side in first and then press firmly on the opposite side until the lid snaps into place.

<figure><img src="../.gitbook/assets/dongle_complete.jpg" alt=""><figcaption><p>Completed CTT Motus dongle, ready for installation in a SensorGnome.</p></figcaption></figure>

**14)** To use with a SensorGnome, you will need a [USB-A to microUSB cable](https://www.amazon.com/6in-Micro-USB-Cable-6-inches/dp/B003YKX6WM), preferably 25cm or less. This may be the same one you used above during firmware installation. Connect to a USB port on the SensorGnome via the USB-A to microUSB cable. The antenna connects to the SMA connector, similarly to a FUNcube dongle.
