~~It's FDE (full-disk encryption).~~ Now it's FBE

[follow this guide](https://xdaforums.com/t/guide-tb-x606f-a-lenovo-m10-plus-fhd-lineage-os-21-gsi-unofficial-android-14.4697538/)

Lineage 19 (android 12) is most compatable with the drivers on this device.

Get vbmeta from C:\ProgramData\RSA\Download\RomFiles after backing up device using Software Fix (Rescue and Smart Assistant) _AKA LMSA_.


Manually wipe data partition, both cache partitions and system. If bootlooping, then something has remained.

Perhaps next time use a ROM with micoG.

## Commands

### Fastboot
```
fastboot getvar all
```

Check magisk version to ensure working: ```adb shell magisk -c```

### TWRP

```
adb reboot bootloader
fastboot flash recovery twrp_X606F_11.img
fastboot --disable-verification flash vbmeta vbmeta.img
fastboot reboot recovery
```

```
fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
```

### Lineage fastboot

```
fastboot devices
fastboot flash system lineage-xx.x-xxxxxxxx-UNOFFICIAL-arm64_bvN-signed.img
fastboot reboot
```

## Perforamance tweaks

* Lower screen resolution from 1200x1900

    ```adb shell wm size 1080x1728```

## Notes

* [Tried Doze-off's Lineage vanilla Security Patch: March 2026](https://github.com/Doze-off/lineage_treble/releases)
    Hangs at lineage boot icon
    Use unxz to extract .xz file

* [Tried MisterZtr 22, 23](https://github.com/MisterZtr/LineageOS_gsi/releases)
    22 hangs at lenovo logo
    23 hangs at lineage logo

* latest lineage 21 by andyyan. I think it's capped by the firmware, reading the last kernel message somewhat confirms this. "Attempted to kill init!" (```adb shell cat /proc/last_kmsg```)
  * ~lineage 21 20250621 bvS works well~ Lineage 21 booted but had driver issues and couldn't go into deep sleep. Lineage 19 (latest gsi version -- android 12) has much better comptability with this device as the drivers supplied by Lenovo are android 11 or 12.

* Changing GSIs reliably
  * Format data + factory reset + advanced remove cache, dcache and system (partly redundany but ensures it works).
  * _adb reboot fastboot_
  * wipr everything 
  * install then go to twrp 
  * install magisk and wipe cache as it asks
    * if doesnt work upload vbmeta and install magisk 
    * reflash vbmeta for some reason (seems like magisk requires this sometimes?)
    * _fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img_
    * Install magisk
  
## Full magisk root

gsi comes with built in root but this is how to get magisk root.

### get boot image in adb shell as root (su)

1. Find where your boot partition is and make a copy to sdcard
```
ls -l /dev/block/by-name/boot

dd if=/dev/block/mmcblk0pXX of=/sdcard/boot.img
```

2. patch the image with magisk, it will most likely put img in sdcard/Download.

3. adb pull that image onto your computer and flash to boot partition using above guide.

## Improve battery life

### Settings

* Disable the following:
  * System > gestures > Life to wake
  * Location > location services > wifi scanning & bluetooth scanning
* phh treble settings
  * Set dynamic fps
  * Remove telephony system (will remove any wakelocks/glitches)
* Developer options
  * Disable always on mobile data

### Apps

* Install deepdoze Magisk module
* Naptime by francisco _made a big difference_ (use uptodown if can't find on app store due to aurora).
  * Set to agressive and disable wifi and location etc when off 


## Fix high power drain

**Couldn't get the tablet to deep sleep so an older android version is required.

I decided to use lineage 19 instead as it uses android 12 and therefore better supports my tablet's drivers.

Some of the steps below relate to trying to get lineage 21 to work**

Lineage gets stuck looking for a radio that isn't there to enable phone calls and sms messaging. It also uses wake locks for ambien display etc.

### Setup not complete

This is due to trying to search for phone services that dont exist in my case

#### Tell the tablet it's finished

```
adb shell settings put global device_provisioned 1
adb shell settings put secure user_setup_complete 1
adb shell settings put global setup_wizard_has_run 1
```

#### Disable it

_You can check your packages (incase you also have google) using: ```adb shell pm list packages | grep setupwizard```_

```
adb shell pm disable-user --user 0 org.lineageos.setupwizard
```

### View wakelocks

```
adb shell dumpsys power | grep -i wakelock

adb shell dumpsys power | grep -A 20 "Wake Locks:"

adb shell dumpsys power | grep mHoldingWakeLockSuspendBlocker
```

### Phone services

```
adb shell pm disable-user --user 0 com.android.phone
adb shell pm disable-user --user 0 com.android.providers.telephony
adb shell pm disable-user --user 0 com.android.server.telecom
adb shell pm disable-user --user 0 com.android.cellbroadcastreceiver
adb shell pm disable-user --user 0 com.android.cellbroadcastservice
```

#### Keeps going away and coming back
```
Wake Locks: size=2
  PARTIAL_WAKE_LOCK              'GsmInboundSmsHandler' ACQ=-350ms (uid=1001 pid=18547)
  PARTIAL_WAKE_LOCK              'CdmaInboundSmsHandler' ACQ=-340ms (uid=1001 pid=18547)
```

run ```adb shell``` and ```su``` for root.


Tell it there is no redio interface: ```resetprop ro.radio.noril yes```

and not voice or sms capable:

**I've realised you actually need full magisk root in order to preserve resetprop commands!!!**

**Every "resetprop" on ro. property sets it live. to persist do the same but using setprop with persist. instead of ro.**
 
**In order to get these set in time, you will need to create a script that magisk will run in /data/adb/service.d/**

```
resetprop ro.telephony.voice.capable false
resetprop ro.telephony.sms.capable false
```

Might atleast remove agressive searching or hint to not look and also no sim :
```
resetprop ro.telephony.default_network 10
resetprop persist.radio.multisim.config ""
```

and soft reboot ```stop; start```


As root, hide com.android.phone so that the system can't keep trying to restart:

```
pm hide com.android.phone
pm hide com.android.providers.telephony
pm hide com.android.server.telecom
```

if ```adb shell ps -u 1001``` gives an "S" then it is waiting in ram and not using a wake lock

```
USER           PID  PPID        VSZ    RSS WCHAN            ADDR S NAME                       
radio         2420  1382   14810304 106808 0                   0 S com.android.phone
```

#### SMS

```
adb shell pm disable-user --user 0 com.android.messaging
```