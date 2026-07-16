# SSH into a SensorGnome

Occasionally it's useful to "SSH into" your SensorGnome so you can run commands directly against the operating system, copy files and logs, and perform other tasks not possible from the SG web interface. Below are two examples for Windows.

## Option 1: Using PuTTY

{% stepper %}
{% step %}
### Connect to your SensorGnome

Refer to the [instructions here](../setup-and-operation/connecting-to-your-sensorgnome.md) if you need a refresher.
{% endstep %}

{% step %}
### Open PuTTY

PuTTY is a free and open source SSH client that can be downloaded on their [website](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) or at the [Microsoft Store](https://apps.microsoft.com/detail/xpfnzksklbp7rj?hl=en-US\&gl=CA).
{% endstep %}

{% step %}
### Enter the SG's hostname or address in PuTTY

<figure><img src="../.gitbook/assets/image (56).png" alt="" width="445"><figcaption></figcaption></figure>

In the example above we accessing the IP address `192.168.7.2` and specifying the username `gnome`. These are the same credentials used to access the SG's web interface. Alternatively, you could use `gnome@sgpi.local`. Leave everything else as default and click `Open` to create a new SSH Session.
{% endstep %}

{% step %}
### Accept the host key

<figure><img src="../.gitbook/assets/image (58).png" alt="" width="563"><figcaption></figcaption></figure>

The host key is a unique "fingerprint" that identifies each device. The first time you connect to an SG, you'll be prompted whether or not you accept this key. If you then connect to a _different_ SG, you'll then be prompted again since the host key saved for this IP address won't match what you already saved.
{% endstep %}

{% step %}
### Enter the SG password

The interface will not display any of the characters you type so it's important to keep track in your head.&#x20;

<figure><img src="../.gitbook/assets/image (60).png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Your SSH session is now active

<figure><img src="../.gitbook/assets/image (59).png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## Option 2: Using Windows Terminal

Most, but not all, newer versions of Windows include SSH capabilities without the need for additional software installation (technically, this is because it often comes with OpenSSH preinstalled.)

{% stepper %}
{% step %}
### Connect to your SensorGnome

Refer to the [instructions here](../setup-and-operation/connecting-to-your-sensorgnome.md) if you need a refresher.
{% endstep %}

{% step %}
### Open Windows Terminal

Windows Terminal is the newer "umbrella" program that contains the older Command Prompt and PowerShell. Any of these will do. Just search for and run Terminal, Command Prompt, or Terminal.
{% endstep %}

{% step %}
### Type `ssh gnome@192.168.7.2` at the prompt&#x20;

With this command, we are initiating an `SSH` session at the IP address `192.168.7.2` and specifying the username `gnome`. These are the same credentials used to access the SG's web interface. Alternatively, you could use `ssh gnome@sgpi.local`.&#x20;

<figure><img src="../.gitbook/assets/image (61).png" alt="" width="497"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Accept the host key

You may be prompted for a host key. The host key is a unique "fingerprint" that identifies each device. The first time you connect to an SG, you'll be prompted whether or not you accept this key. Type `yes` and you'll be prompted for the password.

<figure><img src="../.gitbook/assets/image (62).png" alt="" width="563"><figcaption></figcaption></figure>

#### If the host key has changed...

If you have already connected to an SG at `192.168.7.2` this new host key will not match the key that's been saved and you'll see an error message like the one below.&#x20;

<figure><img src="../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>

Unlike a dedicated SSH client that will prompt you to click whether or not you wish to accept the new key, in Command Prompt and PowerShell, you need to clear the old keys for this IP address using a command:&#x20;

```
ssh-keygen -R 192.168.7.2
```

If you were trying to access the SG at `sgpi.local` you would modify that command to `ssh-keygen -R sgpi.local`.

<figure><img src="../.gitbook/assets/image (67).png" alt="" width="563"><figcaption></figcaption></figure>

Now that the existing key have been cleared, you can return to Step 1 and initiate the SSH session with `ssh gnome@192.168.7.2`&#x20;
{% endstep %}

{% step %}
### Enter the password&#x20;

You'll be prompted for the SG's password, which is the same as the password set during the initial configuration and which is used when accessing the SG's web interface. You can copy and paste the password using the standard Windows shortcut `CTRL-X` but note that you will not see any of the digits, whether you paste or type.

<figure><img src="../.gitbook/assets/image (69).png" alt="" width="482"><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Your SSH session should now be active

<figure><img src="../.gitbook/assets/image (68).png" alt="" width="563"><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

