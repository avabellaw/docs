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

