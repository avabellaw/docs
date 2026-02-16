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

```btrfs filesystem defragment -c```

