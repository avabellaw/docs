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

# Main drive snapshot retention policy
snapshot_preserve_min 2d
snapshot_preserve 6d 3w 6m 1y
snapshot_dir .btrbk-snapshots
snapshot_create onchange

# Backup drive retention policy
target_preserve_min    1d
target_preserve        6d 3w 6m 1y

volume /mnt/btrfsroot
	# root
    subvolume @
        snapshot_preserve	6d 1w 1m 0y
		target_preserve		7d 0w 0m 0y
		target_preserve_min	2d
		target send-receive /media/ava/data-backup/snapshots/root

    # home
	subvolume @home
		target send-receive /media/ava/data-backup/snapshots/home

	# data
	subvolume @data
		target send-receive /media/ava/data-backup/snapshots/home-data

	# steam
	subvolume @steam

```

