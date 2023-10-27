---
description: Notes on upgrading Sensorgnomes
---

# Upgrade notes

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
   ![](<../.gitbook/assets/image (5).png>)
