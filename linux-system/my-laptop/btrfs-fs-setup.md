# All setup and configuration for BTRFS subvolumes (Fedora)

## Precautions

1. Create an immutable subvol inside every important subvol. This will prevent accidental deletion as subvol children have to be deleted first, which requires an intentional permission.
   ```chattr +i /.DEL_LOCK_SUBVOL```

## Backups

### root

Backup hourly with snapper, keep 10. Include ```dnf-plugin-snapper``` for snapshots after installing packages.

Avoiding time based prevents deletion of many snapshots at once on boot.

Backup to second drive using btrfs once a day or less. Keep a week and a month.

### home

Backup every 15 minutes for rollback functionality? 

Maybe keep a numbered amount to prevent deletion of many snapshots at boot. but then have retention as hourly?



## Mounts

### Ram disk

```fstab
# Ramdisk

tmpfs /media/ramdisk tmpfs rw,size=5G 0 0

# Ramdisk for package manager

package_archive /var/cache/dnf tmpfs defaults,size=1G 0 0
```


### Mount options

* nodiscard for all but @Unbacked
  * This is to avoid discard storm after deleting many snapshots. Loads of free space and fstrim is run weekly.
* nodiratime on steam and data 
  * I want to know file access times, I don't mind directory access times.
* noatime on root and unbacked
  * access times not needed at all - saves on read/writes and resources.
* Use zstd and lzo compression as see fit.

## Compress existing data btrfs

**Dont do this on volumes containing snapshots with shared data** - double check this 

```btrfs filesystem defragment -c``` 

## Snapshots

Use btrfs assistant to setup snapshots on boot

dnf-plugin-snapper (python3-dnf-plugin-snapper) will create snapshots before dnf update

### grub-btrfs - view snapshots in grub menu

grub requires older encryption pdkdf2. Fedora will soon have GRUB 2.14 where this won't be an issue. otherwise, luks can't be encrypted with argon2id and use pdkdf2.

[The following steps are from sysguides.com](https://sysguides.com/install-fedora-42-with-snapshot-and-rollback-support)

" git clone https://github.com/Antynea/grub-btrfs
$ cd grub-btrfs

Apply Fedora-specific changes to the configuration file: Copy the entire block from sed to config, paste it into your terminal, and press [Enter] to update the settings.

$ sed -i.bkp \
  -e '/^#GRUB_BTRFS_SNAPSHOT_KERNEL_PARAMETERS=/a \
GRUB_BTRFS_SNAPSHOT_KERNEL_PARAMETERS="rd.live.overlay.overlayfs=1"' \
  -e '/^#GRUB_BTRFS_GRUB_DIRNAME=/a \
GRUB_BTRFS_GRUB_DIRNAME="/boot/grub2"' \
  -e '/^#GRUB_BTRFS_MKCONFIG=/a \
GRUB_BTRFS_MKCONFIG=/usr/bin/grub2-mkconfig' \
  -e '/^#GRUB_BTRFS_SCRIPT_CHECK=/a \
GRUB_BTRFS_SCRIPT_CHECK=grub2-script-check' \
  config

Install it:

$ sudo make install

You might see a "No snapshots found" message initially. This is expected since no snapshots exist yet.

Enable the service:

$ sudo systemctl enable --now grub-btrfsd.service

Your grub-btrfs setup is now finished. You can safely remove the cloned grub-btrfs directory.

$ cd ..
$ rm -rfv grub-btrfs"

#### Install required packages

```sudo dnf install inotify-tools``` - Checks for new snapshots

### Change grub location such as partition or folder dir

```sudo nano /boot/efi/EFI/fedora/grub.cfg```