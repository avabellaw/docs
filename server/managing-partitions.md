# Managing partitions (resize, create, delete)

**cfdisk gives you an interactive view**

## Resize partition

[Detail] (https://geekpeek.net/resize-filesystem-fdisk-resize2fs/)

### Resize BTRFS

```
btrfs filesystem resize -500m/g [mount point]
```

### Check partition first and calc space

**May ask to optimise**

```bash
e2fsck -f /dev/sdaX
```

### Check minimum size filesystem can be shrank too

```bash
resize2fs -P /dev/sdaX
```

#### Use resize2fs to safetly shrink the fs, it will move data if needed

```bash
resize2fs /dev/sdaX "the goal size"G
```

#### To automatically shrink the fs to minimum

```bash
resize2fs -M /dev/sdaX
```

## Move vm disks to another storage place

### Vm config files
```/etc/pve/qemu-server/<VMID>.conf```

```bash
qm move_disk 103 [disk identifier eg scsi0] [new mount] --delete
```

