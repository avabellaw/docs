## Edit boot args

### fedora

Edit /etc/kernel/cmdline

then refresh initrad file

**Rememebr to activate the required lvs here rd.lvm.lv=vg/lv

#### Systemd

check /boot/efi/loader/entries

### Pop OS

Edit using kernelstub 

## Refresh initrad

### fedora

```dracut -f``` to refresh initr file

```sudo kernel-install add $(uname -r) /lib/modules/$(uname -r)/vmlinuz``` to copy over files

### Pop

update-initramfs -k all

## Hibernate

### Fedora

must add ld.lvm.lv=vg/lv to /etc/kernel/cmdline to activate the swap lv.

Then also add resume withe the uuid.
