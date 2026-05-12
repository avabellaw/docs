# Installing adguard

Router needs atleast 50mb of memory, dual-core/quad-core and high enough CPU frequency. linksys-MR7350 has this but not enough disk space / flash memory (100mb overall).

Use a usb to add space, this won't impact performance - only at start up - so don't worry about USB 2.0.

# Mount USB 

__See openwrt docs linked to at bottom__

1. Install ```block-mount e2fsprogs kmod-usb-storage-uas kmod-usb3 kmod-fs-ext4``
2. ```ls -al /dev/sd*```
3. ```mkfs.ext4 /dev/sdaX```
4. Add the usb to automatically fstab ```block detect | uci import fstab```
    * Optionally, edit fstab [mount] target name. use ```uci set fstab.@mount[0].target``` or edit fstab file directly.
5. Enable the new fstab entry and commit ```uci set fstab.@mount[0].enabled='1' && uci commit fstab```
6. Mount device ```/etc/init.d/fstab boot```


# Install

You will want to install manually in order to use a USB. Check CPU type ```uname -w```, remember aarch64 is ARM64. [Choose the appropriate AdGuardHome release](https://github.com/AdguardTeam/AdGuardHome/releases)

# Disabling DNS - port 53 in use

DNSMASQ handles both DHCP and DNS.
[Setting it's port to 0 disables the DNS functionality](https://www.reddit.com/r/openwrt/comments/1lc4aaq/how_to_disable_dnsmasq_local_dns_server/).

In /etc/config/dhcp
```
config dnsmasq
    option port '0'
```

1. Send the file to Openwrt using scp ```scp FILE username@IP:/tmp```
    * If older and doesn't work, use -O to use legacy openssh.
    * Alternatively use wget on the router and extract file from there.
2. Move to USB mount
3. Add execution permission to AdGuardHome/AdGuardHome
4. Install it using ```AdGuardHome/AdGuardHome -s install
    This will create a service under init.d
    Run /etc/init.d/AdGuardHome enable/start

block detect | uci import fstab

# Guides I used

* https://forum.openwrt.org/t/howto-running-adguard-home-on-openwrt/51678
* https://forum.openwrt.org/t/how-to-updated-2021-installing-adguardhome-on-openwrt-manual-and-opkg-method/113904
* https://openwrt.org/docs/guide-user/additional-software/extroot_configuration
* [Quick start for adding usb](https://openwrt.org/docs/guide-user/storage/usb-drives-quickstart)