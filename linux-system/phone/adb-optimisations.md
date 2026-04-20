# Connect to phone and optimise using adb

## Enable USB debugging and connect

1. Enable USB debugging from developer settings.
2. Change to usb tethering (from phone notification after plugging in).
3. run `adb devices` command in terminal, install adb from repo (apt, dnf...) if necessary.
4. Click allow on phone

## Script to run all commands

[See below for just the commands themselves](#optimise-settings-using-adb)

**Currently this script is untested but it's simple enough it should work**

_**DEVICE_NAME** This will just set device name, default device name and synced_account_name_

```bash
DEVICE_NAME=EXAMPLE

declare -a SETTINGS=("global animator_duration_scale 0.2" 
"global transition_animation_scale 0.2"
"global window_animation_scale 0.2"
"global sem_low_heat_mode 1"
"global default_device_name $DEVICE_NAME"
"global device_name $DEVICE_NAME"
"global synced_account_name $DEVICE_NAME"
"global enable_back_animation 1"
"global online_manual_url 0"
"global google_core_control 0"
"secure accessibility_captioning_enabled 0"
"secure accessibility_captioning_font_scale 0.7"
"secure odi_captions_enabled 0"
"secure odi_captions_volume_ui_enabled 0"
"secure bluetooth_name $DEVICE_NAME"
"secure brightness_on_top 1"
"system Flashlight_brightness_level 1001"
"system super_fast_charging 1"
"system system_locales en-GB"
)

for setting in ${SETTINGS[@]}; do adb shell settings put $setting; done

```

## Optimise settings using adb

[Adapted commands from this post on xdaforums](https://xdaforums.com/t/samsung-one-ui-optimization-guide.4376755/post-89683326)

```
Boost Performance
adb shell settings put "global animator_duration_scale 0.2"
adb shell settings put "global transition_animation_scale 0.2"
adb shell settings put "global window_animation_scale 0.2"
adb shell settings put "global sem_low_heat_mode 1"

Configure Captions
adb shell settings put secure accessibility_captioning_enabled 0
adb shell settings put secure accessibility_captioning_font_scale 0.7
adb shell settings put secure odi_captions_enabled 0
adb shell settings put secure odi_captions_volume_ui_enabled 0

Set Device name
adb shell settings put secure bluetooth_name YOURNAMEHERE
adb shell settings put "global default_device_name YOURNAMEHERE"
adb shell settings put "global device_name YOURNAMEHERE"
adb shell settings put "global synced_account_name YOURNAMEHERE"

Enable Predictive Back Gestures
adb shell settings put "global enable_back_animation 1"

Samsung Features
adb shell settings put system Flashlight_brightness_level 1001
adb shell settings put secure brightness_on_top 1
adb shell settings put "global online_manual_url 0"

Settings
adb shell settings put system super_fast_charging 1
adb shell settings put system system_locales en-GB

Google
adb shell settings put "global google_core_control 0"
```