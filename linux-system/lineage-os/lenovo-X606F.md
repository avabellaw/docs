~~It's FDE (full-disk encryption).~~ Now it's FBE

[follow this guide](https://xdaforums.com/t/guide-tb-x606f-a-lenovo-m10-plus-fhd-lineage-os-21-gsi-unofficial-android-14.4697538/)

Get vbmeta from C:\ProgramData\RSA\Download\RomFiles after backing up device using Software Fix (Rescue and Smart Assistant) _AKA LMSA_.


Manually wipe data partition. If bootlooping then something has remained.

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
fastboot flash system lineage-21.0-xxxxxxxx-UNOFFICIAL-arm64_bgN-signed.img
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
  * lineage 21 20250621 bvS works well

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

## Fix high power drain

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