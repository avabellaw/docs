---
title: Btrbk configuration file for fedora
parent: Regularly scheduled btrbk backup
---

## btrbk config file 

The default btrbk config file location is:

```/etc/btrbk/btrbk.conf```

### Current config file

``` conf
timestamp_format long

# Main drive snapshot retention policy
snapshot_preserve_min 6h
snapshot_preserve 6d 3w 1m 0y
snapshot_dir .btrbk-snapshots
snapshot_create onchange

# Backup drive retention policy
target_preserve_min    3h
target_preserve        2d 1w 0m 0y

volume /mnt/btrfsroot
    # root
	subvolume @fedora
		snapshot_preserve	3d 1w 1m 0y
		target_preserve		2d 1w 0m 0y
		target send-receive /media/ava/data-backup/snapshots/fedora-root

	# home
	subvolume @fedora-home
		target_preserve		3h 2d 0w 0m 0y
		target send-receive /media/ava/data-backup/snapshots/fedora-home

	# data
	subvolume @data
		target send-receive /media/ava/data-backup/snapshots/home-data

	# steam
	subvolume @steam

	#root
	subvolume @
	    snapshot_preserve       0d 0w 0m 0y
	    target_preserve         0h 0d 0w 0m 0y
		snapshot_preserve_min latest
	    target_preserve_min latest

		target send-receive /media/ava/data-backup/snapshots/root

	# home
	subvolume @home
	    snapshot_preserve       0d 0w 0m 0y
	    target_preserve         0h 0d 0w 0m 0y
	    snapshot_preserve_min latest
	    target_preserve_min latest

		target send-receive /media/ava/data-backup/snapshots/home

```

