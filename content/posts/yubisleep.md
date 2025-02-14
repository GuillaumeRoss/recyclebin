+++
title = "MacBook YubiKey Induced Insomnia"
date = "2025-02-13"
author = "Guillaume Ross"
description = "Have you ever left a Yubikey in your USB port over the weekend to then leave on Monday with a dead MacBook in your bag?"
+++

Nothing beats realizing your MacBook is out of battery on a Monday morning at the airport when you take it out of the bag. Free adrenaline!

You're not alone, this is widely reported by Mac users.

| Post | Description | Date |
|------|-------------|------|
| [Reddit Discussion](https://www.reddit.com/r/yubikey/comments/lyyrj0/yubikey_nano_5c_drains_mac_book_battery/) | Users report that the YubiKey Nano 5C can cause significant overnight battery drain on MacBooks. | 2021-01-03 |
| [Mac Power Users Forum](https://talk.macpowerusers.com/t/yubico-nano-key-reducing-macbook-pro-power-backup-time/30567) | A thread discusses how YubiKeys may prevent MacBooks from sleeping properly, leading to battery drain. | 2022-09-05 |
| [Reddit: YubiKey 5C NFC Idle Battery Drain](https://www.reddit.com/r/yubikey/comments/kpfag9/yubikey_5c_nfc_idle_battery_drain_on_macbook_air/) | Discussion about idle battery drain on MacBook Air when a YubiKey 5C NFC is plugged in. | 2021-01-03 |

As this issue is often referenced but few details are provided, I decided to post my research mostly to help people searching the web for this issue.


## Cause
After testing, on a 16-inch MacBook Pro with M1 Pro with a battery that's in great condition, and noticing the issue on other MacBooks (Intel and Apple Silicon), I decided to do a bit of research. 

At first, I thought it could be related to [EDR](https://en.wikipedia.org/wiki/Endpoint_detection_and_response) or [DLP](https://en.wikipedia.org/wiki/Data_loss_prevention_software) software.

To rule that out, these tests were done on a Mac with a fresh install done on 15.0, now running 15.3 without any ~~rootkits~~ highly privileged closed source endpoint security tools.

| Time Period | Condition                           | Battery Percentage Lost |
| ----------- | ----------------------------------- | ----------------------- |
| ~12 hours   | No YubiKey                          | 0%                      |
| 15 hours    | YubiKey with FIDO and CCID enabled  | 25%                     |
| 48 hours    | YubiKey with FIDO enabled           | 10%                     |

These tests weren't very scientific, but enough to guess that:
1. The issue is not the key using the power, as I can't really see how such a small device would use 25% of a laptop's battery without even getting warm, so it must be sleep related.
2. [CCID](https://en.wikipedia.org/wiki/CCID_(protocol)) was making things significantly worse. It was enabled as I use [OpenPGP](https://support.yubico.com/hc/en-us/articles/360013790259-Using-Your-YubiKey-with-OpenPGP) on my keys, usually to sign git commits.

To avoid [Yubisneezes](https://www.micahwalter.com/2021/03/yubisneezes/), I had already disabled OTP, which for some weird reason is somehow stil enabled by default on YubiKeys.

A quick look through sleep logs/pmset output showed a variety of events happening roughly every **12 seconds!**, with the laptop sometimes awake for up to 6 seconds.

`2025-02-07 07:00:21.975301-0500 0x2cb806   Default     0x0                  2351   0    PowerChime: 237653.924112: GOING TO SLEEP: kStateDarkWake -> kStateAsleep
`

Here are 7 seconds of `pmset` logs:

```
2025-02-07 07:00:00 -0500 WakeTime            	WakeTime: 0.329 sec                                                        	          
2025-02-07 07:00:00 -0500 Kernel Client Acks  	Delays to Wake notifications: [Codec Output driver is slow(msg: SetState to 1)(77 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(101 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(131 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(134 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(139 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(272 ms)] [Codec Output driver is slow(msg: DidChangeState to 1)(79 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(102 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(132 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(136 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(136 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(276 ms)] [Codec Output driver is slow(msg: SetState to 1)(77 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(103 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(131 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(135 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(136 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(272 ms)] [AppleCS42L84Audio driver is slow(msg: DidChangeState to 1)(77 ms)] [Codec Output driver is slow(msg: WillChangeState to 1)(77 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(103 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(131 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(135 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(136 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(272 ms)] [Codec Output driver is slow(msg: SetState to 1)(77 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(102 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(135 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(134 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(139 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(274 ms)] [Port-USB-C driver is slow(msg: SetState to 1)(51 ms)] [Codec Output driver is slow(msg: WillChangeState to 1)(77 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(102 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(132 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(135 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(140 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(275 ms)] [Codec Output driver is slow(msg: SetState to 1)(77 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(103 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(134 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(136 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(139 ms)] [usb-drd2-port-ss driver is slow(msg: DidChangeState to 3)(274 ms)] [AppleCS42L84Audio driver is slow(msg: DidChangeState to 1)(75 ms)] [Codec Output driver is slow(msg: WillChangeState to 1)(75 ms)] [RTBuddy(GFX) driver is slow(msg: SetState to 2)(102 ms)] [RTBuddy(ANS2) driver is slow(msg: SetState to 2)(135 ms)] [AppleConvergedIPCOLYBTControl driver is slow(msg: SetState to 1)(133 ms)] [AppleMultiFunctionManager driver is slow(msg: SetState to 1)(136 ms)]           
2025-02-07 07:00:00 -0500 Assertions          	PID 445(mDNSResponder) Released MaintenanceWake "mDNSResponder:maintenance" 00:00:00  id:0x0xd000081bd [System: PrevIdle PushSrvc SRPrevSleep kCPU]          
2025-02-07 07:00:02 -0500 com.apple.sleepservices.sessionTerminated	SleepService: window has terminated.                                       	          
2025-02-07 07:00:05 -0500 Assertions          	PID 325(powerd) Released InternalPreventSleep "PM configd - Wait for Device enumeration" 00:00:05  id:0x0xd000081bf [System: No Assertions]          
2025-02-07 07:00:05 -0500 Sleep               	Entering Sleep state due to 'Maintenance Sleep':TCPKeepAlive=active Using Batt (Charge:76%) 2 secs    
2025-02-07 07:00:06 -0500 Wake Requests       	[*process=dasd request=SleepService deltaSecs=960 wakeAt=2025-02-07 07:16:07 info="com.apple.dasd:501:com.apple.duetexpertd.everydayshortcuts"] [process=mDNSResponder request=Maintenance deltaSecs=7198 wakeAt=2025-02-07 09:00:05 info="upkeep wake"] [process=powerd request=TCPKATurnOff deltaSecs=266490 wakeAt=2025-02-10 09:01:36] [process=powerd request=CSPNEvaluation deltaSecs=6463 wakeAt=2025-02-07 08:47:49] [process=powerd request=UserWake deltaSecs=4793 wakeAt=2025-02-07 08:20:00 info="com.apple.alarm.user-invisible-com.apple.calaccessd.alarmEngine.alarm.name,597"]           
2025-02-07 07:00:06 -0500 PM Client Acks      	Delays to Sleep notifications: [com.apple.bluetooth.sleep is slow(1544 ms)]           
2025-02-07 07:00:07 -0500 Assertions          	PID 445(mDNSResponder) Created MaintenanceWake "mDNSResponder:maintenance" 00:00:00  id:0x0xd000081c4 [System: No Assertions]          
2025-02-07 07:00:07 -0500 Assertions          	PID 325(powerd) Created InternalPreventSleep "PM configd - Wait for Device enumeration" 00:00:00  id:0x0xd000081c6 [System: PrevIdle]          
2025-02-07 07:00:07 -0500 com.apple.sleepservices.sessionStarted	SleepService: window begins with cap time=180 secs                         	          
2025-02-07 07:00:07 -0500 DarkWake            	DarkWake from Deep Idle [CDNP] : due to SMC.OutboxNotEmpty smc.70070000 wifibt/ Using BATT (Charge:76%) 7 secs    
2025-02-07 07:00:07 -0500 WakeDetails         	DriverReason:SMC.OutboxNotEmpty - DriverDetails:                           
2025-02-07 07:00:07 -0500 WakeTime            	WakeTime: 0.343 sec                                                        	          
```

### Yubico response

> We have investigated this and have given our results to Apple. The issue is how Apple deals with smart cards. FIDO/PIV/PGP use the CCID interface that uses the smart card service. Apple architecture causes the smart card service to be put to sleep and once a heartbeat goes to the usb device to check, it will then have to reactivate, for smart cards such as PIV and PGP this happens more often than FIDO standard. This however, is all in MacOS software and not related to the YubiKey. There is not a way for us to change our firmware and still function within the standard. Apple has this information but has not provided us with a timeline nor any change or expected solution. 

This issue would therefore also occur with other types of smartcard readers on macOS, but they're rare, especially in form factors someone would leave connected to a laptop. 

### Bug report to Apple
I've submitted a _Feedback_ to Apple, `FB16482480`. Feel free to submit similar ones and even refer to mine. Included were full logs and information around which time periods had the CCID-enabled key connected during sleep.

## Workarounds

There are a few workarounds:

1. Disable CCID, making your key FIDO only, like the [much cheaper model](https://www.yubico.com/products/security-key/)
2. Unplug the key when the laptop is in sleep, completely defeating the purpose of the "nano" form factor.
3. Using hibernation to save battery while making your laptop worse.
4. Using software to "disconnect" the key.


### Making your YubiKey worse

To see if CCID is enabled on your key, run
`ykman info`. 
```
Device type: YubiKey 5C NFC
Serial number: XXXXXX
Firmware version: 5.2.7
Form factor: Keychain (USB-C)
Enabled USB interfaces: FIDO, CCID
NFC transport is enabled

Applications    USB             NFC
Yubico OTP      Disabled        Disabled
FIDO U2F        Enabled         Enabled
FIDO2           Enabled         Enabled
OATH            Disabled        Disabled
PIV             Disabled        Disabled
OpenPGP         Enabled         Disabled
YubiHSM Auth    Not available   Not available
```

If you do not have `ykman` (it's in brew), you can run `pmset -g assertions`.

```
id=567  level=255 0x4=USB creat=2025-02-14, 10:07 description=com.apple.usb.externaldevice.02100000 owner=YubiKey FIDO+CCID
```

If CCID (and possibly OTP) are enabled, and you do not need them, disable them by disabling protocols on the USB interface until only FIDO2 and FIDO U2F are enabled.

This can be done with `ykman`:
```
ykman config usb --disable OpenPGP
```

Alternatively, [YubiKey Manager](https://www.yubico.com/support/download/yubikey-manager/) provides a GUI for this. Once you're done, check if CCID is enabled again. It shouldn't be.

### Making your life worse
Try to remember to unplug your nano key from the laptop before putting it in a bag the night before a trip!
This is what I do, but considering I switch my keys from laptop to laptop so often, it's not so bad. 

This is more annoying for people who have a key dedicated to a "work laptop", for example.

### Making your laptop worse

This was the workaround provided to me by Yubico, to make the laptop hibernate instead of sleep.

```
sudo pmset -a hibernatemode 25
sudo pmset -a standbydelay 15 
```

### Running more privileged software

I don't love this option simply because I'd rather not run software when I can avoid it, especially when it requires such levels of access.

That being said, [Yubiswitch](https://github.com/pallotron/yubiswitch) looks like a very neat application for giving you the ability to automatically disable YubiKeys, enable them with keyboard shortcuts etc. 


### Untested: Disable Bluetooth Wake and PowerNap  

It is possible that disabling these features on macOS might improve the situation, based on the wakes seemingly being associated to Bluetooth. As someone constantly using laptops with external keyboards, this is a non-starter for me.

## What's next?

Probably nothing. I'll keep removing my keys overnight, and Apple will likely not acknowledge my report, but at least there's now a page online with a bit more detail than the previously linked forum threads!

{{< goatcounter >}}