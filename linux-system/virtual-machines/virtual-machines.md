# Virtual machines (KVM/QEMU)

## Storage

### Type

Use virtIO for best performance.
You will need to install drivers in guest.

### Convert disks

```-p``` Is to show progress

#### qcow2 to raw

```-S 0``` is to skip 0s (trim essentially).

```qemu-img convert -p -f qcow2 -O raw -S 0 source_image.qcow2 /dev/vgname/vm-disk```

```dd if=/var/lib/libvirt/images/windows.raw of=/dev/vg0/windows-128k bs=128K status=progress conv=sparse```


#### Convert raw to qcow2 

```qemu-img convert -p -f raw -O qcow2 /dev/<volume-group>/<logical-volume> /path/to/storage/image.qcow2  ```
