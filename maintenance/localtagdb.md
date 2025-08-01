---
description: >-
  You can view live detections of your project's Lotek tags on the SensorGnomes
  web interface by following these steps.
---

# Uploading a local tag database

{% hint style="warning" %}
If you are trying to upload a tag database to a SensorStation, or a V1 SensorGnome, refer to the [V1 SG guide](localtagdb.md).
{% endhint %}

## Introduction

If you are installing a station that detects Lotek tags, it is often useful to know whether it is able to detect tags in real time, particularly if you are deploying tags in the vicinity. Thankfully this is possible by loading a local tag database on to the internal storage of the device. When the SensorGnome boots up, it checks for a tag database and will display any of those tags it "hears" on its web interface.&#x20;

Viewing your live tag detections can also be useful when deploying tags or as a make-shift manual tracking device when a Lotek receiver is unavailable.

This works by using a local version of the tag finder algorithm ([`find_tags_unifile`](https://github.com/MotusWTS/find_tags)) in comparison with the tag recordings provided during registration. Note that this local version differs from the version found on Motus, mainly in that Motus searches for tags from all projects across the network (but only those known to be actively deployed during the given time period) whereas this method will only compare raw radio data with the tag patterns it has been provided, regardless of deployment period.

### Steps

1. Navigate to your [project's tag management pages ](http://motus.org/data/project/tags) and click the "Download tag database" button

![](../.gitbook/assets/2025-08-01_143706.png)

2\. Download the first option. This will contain all the tags currently registered to your project.

<figure><img src="../.gitbook/assets/2025-08-01_144708 (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
The additional options seen above are an earlier, depcreated, format that was summarized by yearly quarter. These will only be visible for older projects, and will almost never be needed.
{% endhint %}

3. Connect to your SensorGnome via the WiFi hotspot and navigate to the "Radios" tab. Click "Upload new tag database" and select the `.sqlite` file you just downloaded. You should now see an updated summary including the new project and the number of tags you added.

{% hint style="warning" %}
You can add `.sqlite` files from multiple projects without needing to merge them into one.
{% endhint %}

<figure><img src="../.gitbook/assets/2025-08-01_145413.png" alt=""><figcaption></figcaption></figure>
