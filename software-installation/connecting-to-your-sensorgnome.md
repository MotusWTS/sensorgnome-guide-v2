# Connecting to your SensorGnome

In order to check the status of a SensorGnome, download data, or modify the configuration, you must first connect to it using a laptop or phone. The primary method is by connecting to a WiFi hotspot that the SG broadcasts.

{% hint style="info" %}
You can use either a laptop of smartphone to connect to a SensorGnome. However, due to the larger screen for working, ease of downloading files, taking full-page screenshots, and other benefits, a laptop is usually preferred if available.
{% endhint %}

## The SensorGnome's WiFi hotspot

By default, a SensorGnome broadcasts a WiFi hotspot whenever it is powered on, assuming it is functioning normally. The WiFi hotspot _only_ serves to establish a connection between the SG and your computer or phone; it does not connect the SG to the internet.&#x20;

#### WiFi network name and password

The WiFi hotspot network name corresponds to the SensorGnome's unique serial number, e.g. `SG-3BEERPI36FDA`. The WiFi password is set when the SensorGnome is configured immediately after a fresh software installation. If the SG's WiFi hotspot name ends with a `-init` suffix, it has not been initialized and does not yet have a password.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption><p>Several SensorGnomes are running and within WiFi range of this location. The last SG, with the <code>-init</code> suffix following the serial number, has not yet been initialized.</p></figcaption></figure>

1. Connect to the WiFi hotspot of the SG you want to connect to
2. Enter the password when prompted

You should now be connected to the SG via its WiFi hotspot. But because this network does not provide access to the internet, you'll see something similar to the screenshot below (on a Windows 10/11 computer). This is prompting you to log in to the network as you would with an airport or hotel network. This is referred to a **captive portal,** and is the method of accessing the SensorGnome's web interface (also referred to as "Web UI" or just "UI").

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption><p>The hotspot doesn't provide access to the internet, but the "login" page (the captive portal) is how you access the SG web interface.</p></figcaption></figure>

3. Click the "Open browser and connect" option for the WiFi network to arrive at the following page

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption><p>Click on either of the links to bring you to the SG web interface log in page</p></figcaption></figure>

On may laptops, your browser may automatically bring you to this page, or prompt you to this network" message, or similar, which serves the same purpose.&#x20;

4. Click on either of those two links to arrive at the web interface. You can also type them in directly to your browser.

* [`http://192.168.7.2:88`](http://192.168.7.2:88/) OR
* [`http://spgi.local:88`](http://spgi.local:88/)&#x20;

4. &#x20;You should now see the page below allowing you to log in and access the SensorGnome web interface. The username is already pre-populated as `gnome`. **The password is the same as the WiFi password you just used to connect to the hotspot.**&#x20;

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption><p>SG log in dialog. The username is always gnome, and the password is the same as the WiFi hotspot password.</p></figcaption></figure>

Once logged in, you'll see something like this:

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption><p>The SensorGnome Web Interface</p></figcaption></figure>

### **Troubleshooting and tips**

The hot-spot acts as a "captive portal", which means that it looks to your computer/phone like a WiFi network typical of airports or hotels where you have to enter credentials or accept an agreement to get internet access. However, the SensorGnome never actually provides internet access...

Some of the issues users have encountered are:

* a web browser automatically opens to show the SensorGnome's web UI, but it may be a special system browser and not your regular browser (on iOS that browser seems not to show certain charts)
* your computer/phone may become "impatient" about not getting internet access and decide to switch back to your regular WiFi
* if using Android, your phone may use the cellular network instead of the Hot-Spot and you most likely have to turn cellular/mobile off

In many cases the hot-spot "just works" but being aware of the potential issues helps.

If you get stuck and "it doesn't work" check that your computer/phone is still connected to the hot-spot. You can also connect to the SensorGnome using your favorite browser at `http://192.168.7.2` to avoid the system browser.

When a device connects to the hot-spot it detects that there is no internet connection (because the hot-spot only provides access to the SensorGnome ). It then assumes that this is a "captive portal", which means that the user has to connect to a specific web site to log in or agree to some legal terms before getting internet access. This is typical of WiFi in public locations, e.g. airport, hotel, or coffee shop.

In order to facilitate the required login or acceptance of terms your device starts a web browser which connects to the SensorGnome's web UI. This usually works well and allows you to use the UI. However, depending on the device and on other available WiFi networks you may run into issues. The main cause for issues is that your device expects to eventually get internet access but the SensorGnome will never provide that, so your device may take actions in an effort to restore internet access. Specifically, your device may decide to disconnect from the SensorGnome hot-spot and connect to some other network, such as a previously working one.

Tips:

* If your device prompts you with a message stating that this network does not provide internet access and whether you want to stay connected anyway choose the option to stay connected.
* If the web UI stops working, check whether your device disconnected from the hot-spot and connected to a different network. If so, reconnect to the hot-spot.
* The web browser used in the captive portal mode (i.e. to allow you to log-in or accept terms) may not be the regular web browser you use on your device. If it does not work well or is closed on you open your standard browser and try `http://192.168.7.2` or `http://sgpi.local`.
* If the captive portal ends up being broken or too confusing turn it off on the landing page at `http://192.168.7.2`, `http://sgpi.local`. Then possibly disconnect and reconnect to the hot-spot and navigate to one of the those two URLs explicitly by bringing up a browser. Your device operating system will warn about "no internet access" and some "stay connected anyway" setting may be necessary.

## Connect to your SensorGnome via Internet

If your SensorGnome and your laptop/phone are both connected to the internet AND on the same WiFi or Ethernet network, you can connect to it this way. This can be helpful in cases where the WiFi hotspot method described above is not working.&#x20;

1. First: verify that your SG's green LED shows 3 flashes every 2 seconds. If it doesn't, your RPi SG does not have internet access.
2. Open the SensorGnome Hub (SGhub) website at [https://www.sensorgnome.net](https://www.sensorgnome.net), logging in with your Motus user name and password when prompted.
3. The table at the top shows the most recently initialized SensorGnomes: yours may already be at the top! You can identify it using the label column which should show the hostname you entered.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

4. Click on the row with your SensorGnome, this switches to the station tab; at the top-right is a link to the web UI:

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

5. If you have already configured your SG with a password, enter it when prompted. If this SG has not yet been initialized, you'll have the opportuniyt to set its password here.&#x20;

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

You are now connected to the web UI:<br>

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
