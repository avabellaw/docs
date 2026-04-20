## Installation of OpenWrt on Linksys-MR7350

[OpenWrt docs](https://openwrt.org/inbox/toh/linksys/mr7350_1.0)

[Firmware download](https://firmware-selector.openwrt.org/)

**MR7350 is version 1 if doesn't specify version number**

Ensure no symbols in admin password incase it causes problems.

### Upload firmware

**The firmware update/upload "choose file" button is missing.**

[Go to /fwupdate.html to uncover it](https://192.168.1.1/fwupdate.html)

_[Found thanks to goofust on Reddit](https://www.reddit.com/r/openwrt/comments/1jsxhnw/i_dont_know_how_to_install_openwrt_on_linksys/)_

### Non-overlapping kernel and rootfs partitions (referenced in above OpenWrt docs)

dmesg | grep -A30 "MTD partitions"

My router is the version that has non-overlapping partitions.