*TrueNas requires the serial number too, use lsblk to identify disks and get this*

```lsblk --output NAME,UUID,ID,SIZE,LABEL,SERIAL```


```qm set [VM number] -scsi2 /dev/disk/by-id/[disk-id],serial=[disk serial number]```

For each disk set a different -scsi var. -scsi0 will be the boot disk then increment for each disk (-scsi1, -scsi2).