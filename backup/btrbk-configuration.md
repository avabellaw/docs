---
title: Btrbk configuration file
parent: Regularly scheduled btrbk backup
---

## btrbk config file 

The default btrbk config file location is:

```/etc/btrbk/btrbk.conf```

### Current config file

``` conf
timestamp_format long

# Backup drive retention policy
target_preserve_min    3d
target_preserve        7d 4w 6m 1y

# Main drive snapshot retention policy
snapshot_preserve_min 14d
snapshot_preserve 7d 4w 6m 1y

volume /mnt/btrfsroot
    snapshot_dir .btrbk-snapshots

    subvolume @
    target send-receive /media/ava/data-backup/snapshots/root

    subvolume @home
    target send-receive /media/ava/data-backup/snapshots/home
```

