+++
title = "Using HomeKit Devices Across VLANs and Subnets"
date = "2023-05-29"
author = "Guillaume Ross"
description = "Using HomeKit Devices Across VLANs and Subnets"
+++

This article was originally posted on [Medium](https://medium.com/@gepeto42/using-homekit-devices-across-vlans-and-subnets-aa5ae1024939). As I am removing my Medium articles, this will be the archive for this content. 

Originally written on September 8, 2017.

_TL;DR Version: Make sure your iOS devices can connect to the HomeKit Devices on port 80 and 443, and replicate mDNS from VLAN/Subnet to VLAN/Subnet with Avahi._

So, you’ve setup multiple VLANs and SSIDs at home, you’re keeping all those Internet Connected toasters away from the LAN where your crown jewels sit.

Good! Except, woops, a ton of stuff is broken?

HomeKit uses the [HAP Protocol](https://developer.apple.com/support/homekit-accessory-protocol/), which actually uses peer-to-peer connectivity for really fast action when you try to perform actions. If your device is unable to reach the HomeKit device, it will, through iCloud, try to perform the HomeKit action through your AppleTV.

If your AppleTV is unable to reach the HomeKit device, the request will fail.

If your AppleTV is in the right subnet and DOES reach it, it will work, however, it will be slower than if it had worked directly from your device. And believe me, 500ms of lag when trying to turn on the lights can be a significant pain.

Here is how to get it working almost as if it was on the same network.

**Note**: This is not the most restrictive configuration possible. I am sure you can lock it down more, especially if everything you have has known hostnames or assigned IPs that do not change.

1. Allow your main LAN to connect to port 80 and 443 on HomeKit devices. You can do this via IP to IP + port rules, or, if you do not mind your main network reaching the IoT network, simply allow 80 and 443 from Main to IoT LANs. You can make this more restrictive by only allowing the static or reserved IPs of devices you use with HomeKit. Be sure to include your AppleTV or iPad so your schedules work when you’re away.
2. Disable Guest Isolation on your IoT SSID while you pair the devices (the pairing process seems to require p2p connectivity for some devices, and in any case, some apps will force you to send Wi-Fi settings from your phone, preventing you from being on the main SSID while you do this).
3. Join your IoT SSID with your phone.
4. Add the device to HomeKit. I tested this with iDevices devices (Ferrari LaFerrari anyone?). Their process for adding devices is very brittle, and I had a lot of issues, and had to try again many times. I had to do that the first time I set them up on a single Wi-Fi and VLAN however.
5. Install [Avahi](https://avahi.org/) on your Firewall, or on any device that sees both the Main and the IoT VLAN. It can be made to work on many devices, but of course if you are using pfSense it is available as a [package](https://doc.pfsense.org/index.php/Avahi_package).
6. Enable Avahi, and make sure it is listening on your IoT and your Main interface (deny it everywhere else). This is where you could potentially lock this down more: create custom rules and settings so it only works from specific IoT devices. In my case, it wasn’t worth it.
7. Reconnect your phone to your main SSID.
8. Turn the lights on and off and on and off and on and off. And on and off and on and off. **It works!**
9. **Re-enable guest isolation on your IoT SSID**. Do not forget this step, you don’t want your toaster to hack your fridge do you? _Note: Some APs are very hardcore with their guest isolation, and will block traffic from/to any RFC1918 address. If enabling guest isolation breaks your previously working setup, look to see if you can create custom ACLs on your AP. If not, I would recommend creating an SSID only for HomeKit devices, and leaving guest isolation disabled. In some cases, you will need to whitelist your Main LAN from it completely, but this should be filtered by your firewall anyway. On some, such as Ubiquiti UniFi gear, you need to allow the guest isolated SSID to talk to 224.0.0.251 for mDNS._