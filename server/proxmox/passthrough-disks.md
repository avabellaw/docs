```lsblk --output NAME,UUID,ID,SIZE,LABEL,SERIAL```


```qm set [VM number] -scsi2 /dev/disk/by-id/[disk-id],serial=[disk serial number]```

For each disk set a different -scsi var. -scsi0 will be the boot disk then increment for each disk (-scsi1, -scsi2).

## TrueNas

*TrueNas requires the serial number too, use lsblk to identify disks and get this*

You may need to wipe and add new partition table.

1. blkdiscard /dev/* will instantly mark data as unused
2. fdisk /dev/* - interactive prompt will open
3. Type 'g' for gpt
4. Type 'n' for new partition
5. Type 'w' for write