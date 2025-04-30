---
description: Notes on upgrading Sensorgnomes
---

# Software upgrade

In order to upgrade a Sensorgnome an Internet connection is required as the Sensorgnome pulls packages from a server on the Internet. Some thoughts about upgrades:

* if it ain't broke, don't fix it
* the standard upgrade process only updates Sensorgnome software, almost all system software is left unchanged, this avoids ending up with a completely non-functional system due to a problem during the upgrade
* it is possible to perform a full system upgrade, but perhaps swapping in an SD-card with a fresh clean image is a better option, in any case, unless there is a specific problem this is supposed to fix see the first bullet point...
* If updating with a fresh image, be sure to [update the boot number](upgrade-notes.md#updating-the-boot-number) to maintain the proper sequence.

## Upgrade Process

The upgrade process is managed using the SG Web UI's software tab shown below. The functionality first needs to be enabled with the `enable buttons` toggle. The next step is to run a software update check (red `check` button). In the situation shown one Sensorgnome package can be updated and many operating system packages. The red `sg upgrade` button starts the upgrade of just the Sensorgnome packages, which is the recommended action. Upgrading all the OS packages can have unintended issues.

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

## Upgrading from V2.0-RC12 or prior to later versions

Due to the expiration of the software repository signing key manual intervention is required in order to upgrade from V2.0-RC12 or prior (version 2023-XXX on the software tab) to the latest versions. In order to update the repository key the following command must be executed on the Sensorgnome (i.e. SSH to the Sensorgnome):

`sudo curl -L -o /etc/apt/trusted.gpg.d/sensorgnome.gpg` [`https://sensorgnome.s3.amazonaws.com/sensorgnome.gpg`](https://sensorgnome.s3.amazonaws.com/sensorgnome.gpg)

_Note that this command is one line even though it may appear wrapped in this manual!_

After issuing this command (you should see messages showing the key being downloaded) the regular upgrade procedure will work.

## Upgrading from v2.0-RC8 or v2.0-RC6

This upgrade requires patience because the code in those versions could not deal with some of the changes that are required and thus a "double-upgrade" happens under the covers. Please follow this process:

1.  On the software tab, "enable changes"\


    <figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
2.  Run "check", have a minute of patience and you should see available updates. If it doesn't seem to complete run "check" again

    <figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
3.  Run "sg-upgrade" and monitor the log (or get a tea/coffee for 2-3 minutes), after a couple of minutes the log will seem to be done (scroll down in the widget), showing:\


    <figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
4.  Wait another minute and you may notice the green LED on the rPi stop blinking for a bit as it reboots, then reload your browser tab (you may have to reconnect to the hot-spot if you're using that to access the Sensorgnome), you should see a prompt to verify the SG password, if not, wait another minute and reload the browser tab again:\


    <figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
5.  After entering the password the web UI should load, however, it may take a while (20-30 secs) and you may have to reconnect to the hot-spot again (sorry). On the software tab confirm that you are now running version 2023-115 or later\


    <figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## Updating the boot number

### What is the boot number? <a href="#what-is-the-boot-number" id="what-is-the-boot-number"></a>

The boot number is an incremental marker of how many times a SG has rebooted. It is represented in the file name as well as the web interface in the V2 SensorGnome.

<figure><img src="https://docs.motus.org/~gitbook/image?url=https%3A%2F%2F2642021313-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FGrf8Wg5oUFTJbWHxHdF7%252Fuploads%252FIjBEAMxz01yJwShYnfKj%252F2025-01-22_112412.png%3Falt%3Dmedia%26token%3Dd37b489b-1ce5-4824-9914-9433af9ea1cd&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=42063987&#x26;sv=2" alt=""><figcaption><p>The current boot number displayed in the V2 SG web interface</p></figcaption></figure>

<figure><img src="https://docs.motus.org/~gitbook/image?url=https%3A%2F%2F2642021313-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FGrf8Wg5oUFTJbWHxHdF7%252Fuploads%252FynjelAHd1hj8qke1XhsS%252F2025-01-22_115538.png%3Falt%3Dmedia%26token%3D01992b43-3968-4c30-9e6e-f39bb1db4d13&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=fb782d40&#x26;sv=2" alt=""><figcaption><p>The boot number is embedded in the detection data file name (on both V1 and V2 SG)</p></figcaption></figure>

The boot number is an essential component when processing detection data as it can account for bad timestamps (when the GPS reports an incorrect date/time) and for stringing together long detection runs that overlap multiple uploaded batches. Because the `tagfinder` algorithm works under the assumption that the boot number _always_ increments, issues can arise when the boot number is reset.

### When is the boot number reset? <a href="#when-is-the-boot-number-reset" id="when-is-the-boot-number-reset"></a>

When upgrading a SensorGnome's software with a _newly flashed_ SD card, the boot number in the file names will be reset to `0`. This applies to V2 SG with a card that's been flashed with the software, V1 SG where the software files have copied to a new card, and SG running on BeagleBones. Since this will violate the assumption that the boot number always increments, this may result in missing or false detections until the [receiver data is reprocessed](https://docs.motus.org/en/about-motus/how-data-are-processed/reprocessing-receiver-data).

{% hint style="info" %}
If updating a V2 SG over the internet using the web interface, the proper boot number sequence is retained.
{% endhint %}

### How to update the boot number <a href="#how-to-update-the-boot-number" id="how-to-update-the-boot-number"></a>

{% hint style="info" %}
Updating the boot number is only possible on the V2 SensorGnome software running on a Raspberry Pi.

If you cannot, or prefer not to, run V2 SG on a Raspberry Pi, the boot number will only be corrected after the receiver data is reprocessed. You can read more about reprocessing receiver data [here](https://docs.motus.org/en/about-motus/how-data-are-processed/reprocessing-receiver-data).


{% endhint %}

1. **Before updating the software**, connect to the SensorGnome and visit the web interface. From the landing page, you should be able to see the current boot number (as the screenshot above shows). Write this number down or otherwise commit it to memory.
2. After you have upgraded and initiated the new software, connect to the SG and navigate to the "Software" tab to update the boot number. Recalling the previous value, set the new number higher higher than necessary then reboot the SG and confirm that the change has persisted when you connect again.&#x20;
3. Reboot the SG and connect to it to confirm that the change is persistent.

<figure><img src="../.gitbook/assets/2025-01-22_112412.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
When a receiver’s data is reprocessed (also referred to as re-running), the correct boot number sequence is restored and any spurious or missing detections resolved – assuming all tag deployment metadata is up-to-date. You can read more about receiver data reprocessing [here](https://docs.motus.org/en/about-motus/how-data-are-processed/reprocessing-receiver-data).
{% endhint %}

