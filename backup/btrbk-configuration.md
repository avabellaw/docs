---
title: Btrbk configuration file
parent: Regularly scheduled btrbk backup
---

The default btrbk config file location is:

```/etc/btrbk/btrbk.conf```

Create or edit this file and add the following:

timestamp_format long

# Backup drive retention policy
target_preserve_min    3d
target_preserve        7d 4w 6m 1y

# Main drive snapshot retention policy
snapshot_preserve_min 28d
snapshot_preserve 7d 4w 6m 1y

volume /mnt/btrfsroot
    snapshot_dir .btrbk-snapshots
    target send-receive /media/ava/data-backup/root
    subvolume @
    subvolume @home

