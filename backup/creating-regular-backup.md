---
title: Regularly scheduled btrbk backup
parent: Backup with btrbk
---

# Create regular backup to seperate drive using btrbk


## Systemd service unit files

**symlink systemd-unit files using ```systemctl link [absolute path to dir]```**

* [btrbk.service](systemd-units/btrbk.service)
* [btrbk.timer](systemd-units/btrbk.timer)
* [mnt-btrfsroot.mount](systemd-units/mnt-btrfsroot.mount)

Creates snapshots on source drive, then creates an incremental backup on the dest drive.

## Configuring the backup

Install btrbk 

Edit the conf file /etc/btrbk/btrbk.conf

You will find the example here.

## Setting up systemd timer for hourly backups

The example files are in the systemd-units dir.

.mount is used by .service to mount the btrfs root to /mnt/btrfsroot temporarily. This gives btrbk access to the btrfs subvolumes.

.service executes the btrbk run command.

.timer is the systemctl timer that starts the .service every hour.

add the timer using the command: systemctl enable --now btrbk.timer

--now makes it start the task straight away.

systemctl list-timers will list all of the enabled timers and will should you when last ran.
