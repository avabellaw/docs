# DISREGARD - NEEDS UPDATING Enable secure boot

## Shim

Shim is a mini bootloader trusted by Microsoft to load bootloaders. It is not presigned and requires the user to sign.

## Shim-signed

Package that contains an already signed shim binary. It uses keys trusted by Ubuntu.

  One such trusted key is systemd-boot*

## Pop os

```sudo apt install shim-signed```

shim signed should be installed under /boot/efi/EFI/BOOT

1. Make a backup of systemd-bootloader 
2. Copy and renmae systemd-bootloader to "grubx64.efi" to allow shim to recognise it
3. Move the newly created copy to the same dir as shim in /EFI/BOOT
4. Set shim-signed as the default bootloader

```
sudo efibootmgr --disk [disk without the part eg /dev/nvme0n1] --part [parititon number eg. 5] \
  --create --label "Shim" \
  --loader '\EFI\BOOT\BOOTX64.EFI'
```

sudo efibootmgr --disk /dev/nvme0n1 --part 5 \
  --create --label "Shim" \
  --loader '\EFI\BOOT\BOOTX64.EFI'