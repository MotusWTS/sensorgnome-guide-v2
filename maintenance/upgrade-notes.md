---
description: Notes on upgrading Sensorgnomes
---

# Software upgrade

In order to upgrade a Sensorgnome an Internet connection is required as the Sensorgnome pulls packages from a server on the Internet. Some thoughts about upgrades:

* if it ain't broke, don't fix it
* the standard upgrade process only updates Sensorgnome software, almost all system software is left unchanged, this avoids ending up with a completely non-functional system due to a problem during the upgrade
* it is possible to perform a full system upgrade, but perhaps swapping in an SD-card with a fresh clean image is a better option, in any case, unless there is a specific problem this is supposed to fix see the first bullet point...

## Upgrade Process

The upgrade process is managed using the SG Web UI's software tab shown below. The functionality first needs to be enabled with the `enable buttons` toggle. The next step is to run a software update check (red `check` button). In the situation shown one Sensorgnome package can be updated and many operating system packages. The red `sg upgrade` button starts the upgrade of just the Sensorgnome packages, which is the recommended action. Upgrading all the OS packages can have unintended issues.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## Upgrading from V2.0-RC12 or prior to later versions

Due to the expiration of the software repository signing key manual intervention is required in order to upgrade from V2.0-RC12 or prior (version 2023-XXX on the software tab) to the latest versions. In order to update the repository key the following command must be executed on the Sensorgnome (i.e. SSH to the Sensorgnome):

`sudo curl -L -o /etc/apt/trusted.gpg.d/sensorgnome.gpg` [`https://sensorgnome.s3.amazonaws.com/sensorgnome.gpg`](https://sensorgnome.s3.amazonaws.com/sensorgnome.gpg)

Note that this command is one line even though it may appear wrapped in this manual!

After issuing this command (you should see messages showing the key being downloaded) the regular upgrade procedure will work.

## Upgrading from v2.0-RC8 or v2.0-RC6

This upgrade requires patience because the code in those versions could not deal with some of the changes that are required and thus a "double-upgrade" happens under the covers. Please follow this process:

1. On the software tab, "enable changes"\
   ![](<../.gitbook/assets/image (15).png>)
2. Run "check", have a minute of patience and you should see available updates:\
   ![](<../.gitbook/assets/image (18).png>)\
   if it doesn't seem to complete run "check" again
3. Run "sg-upgrade" and monitor the log (or get a tea/coffee for 2-3 minutes), after a couple of minutes the log will seem to be done (scroll down in the widget), showing:\
   ![](<../.gitbook/assets/image (9).png>)
4. Wait another minute and you may notice the green LED on the rPi stop blinking for a bit as it reboots, then reload your browser tab (you may have to reconnect to the hot-spot if you're using that to access the Sensorgnome), you should see a prompt to verify the SG password, if not, wait another minute and reload the browser tab again:\
   ![](<../.gitbook/assets/image (12).png>)
5. After entering the password the web UI should load, however, it may take a while (20-30 secs) and you may have to reconnect to the hot-spot again (sorry). On the software tab confirm that you are now running version 2023-115 or later\
   ![](<../.gitbook/assets/image (5) (1).png>)
