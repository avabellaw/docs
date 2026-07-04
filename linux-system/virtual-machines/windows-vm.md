# Windows VM setup

https://gist.github.com/KitBaroness/e361cae0600003bfb8ecehttps://gist.github.com/KitBaroness/e361cae0600003bfb8ecea89c56d1f2aa89c56d1f2a

https://technologytales.com/windows-11-virtualisation-on-linux-using-kvm-and-qemu/

[WinFSP for virtio] (https://winfsp.dev/)

[How to properly install win11 on kvm / optimse windows](https://sysguides.com/install-windows-11-on-kvm), video version: [https://www.youtube.com/watch?v=q8ZsO-h14Po&t=35s](https://www.youtube.com/watch?v=q8ZsO-h14Po&t=35s)
  * Has specific cpu configurations for intel/amd

Decided to reimplement LVM thin but this time it's a dedicated partition as my main drive didn't required lvm anyway.

I will:

* Create luks volume and lvm-thin underneath it
* Disable LVM zeroing to improve performance as currently windows is the only VM
* Reduce LVM blocksize from 512KB to 64KB (lowest possible) to more be close to Window's 4KB
* ~~Sacrifice some diskspace and increase Windows blocksize 4KB to 32KB  (maybe even 6KB to match)~~
  Set windows blocksize to 64kb because it's wasting 6kb through lvmthin anyway.
  * Disables compression but can use zstd on the hypervisor
  * Bitlocker still works

## Reasons for original switch from lvm thin

**Switched from lvm-thin to qcow2 on btrfs (CoW disabled)**

Mismatch of technologies, windows flushing small amounts of data (4kbs) means (512kb) blocks still.

Not enough cores to make feasible on my laptop either.

Opting for qcow2 on btrfs, this is near native anyway. Besides qcow2 is highly optimised so it's very little performance reduced.

I wanted a reason to keep lvm but I will be removing it and sticking with just btrfs. 
I liked the flexibility and learning oppurtunity from using lvm, however, I've learn a lot now anyway and there is no reason to continue with the added complexity and performance hit.

Now using raw disk on LVM-Thin for performance gains while maintaining ease of use. (Helps that it gives me a reason to keep LVM). [See considerations](../virtual-machine-considerations.md)

## Troubleshooting

### Disks

Trim may not work if disk granularity is mismatching. Needs to match the minimum blocksize for lvmthin to accept.

Either add ```<blockio discard_granularity="65536"/>``` to disk or add the following to domain xml in virt manager to set the disk granularity to the blocksize of the lvm thin (add an alias to target disk _has to have "ua-" prepended_):
```xml
<qemu:override>
    <qemu:device alias="ua-windows-disk">
      <qemu:frontend>
        <qemu:property name="discard_granularity" type="unsigned" value="BLOCKSIZE"/>
      </qemu:frontend>
    </qemu:device>
  </qemu:override>
```

Add xml schema to domain args: ```xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0"```



```fsutil fsinfo sectorinfo C:``` info/trim info
```fsutil behavior set DisableDeleteNotify 1```
```fsutil behavior set DisableDeleteNotify 0```
```winsat disk -drive c```


## My laptop

### CPUS

* 10 cores and 2 threads.
  * Threads from hyperthreading so should only assign max 10 cores with 1 thread each **apprently**
    * I'm going to set 5 cores, 2 threads according to one guide.
* Add rotation_rate=1 to <target/> parameters on SCSI disk.
* [Install spice guest tools on windows](https://www.spice-space.org/download.html)

```lscpu -e``` see performance / energy efficient cores

for CPU pinning check out: https://leduccc.medium.com/improving-the-performance-of-a-windows-10-guest-on-qemu-a5b3f54d9cf5

Best to not mix P and E threads. Best would be to use cpu 3 and 4 performance threads, however, as I am using power bi that uses multi-threading I will use all available E cores. 

Since it's best to not mix performance and engergy cores, I will assign the iothread 2 performance cores for symeltaneous read write processing.

Need to update iothread from 1 to <iothreads>2<iothreads>

Need to add 2 queues to disk ```<driver queues="2"/>``` within virtio scsi controller config so it knows to use them.

### Disabling zeroing on vm-pool

Currently I only have windows in said pool so disabling this will improve performance and reduce wear.

```sudo lvchange -Zn vg0/vm-pool```

### Set windows allocation size **Windows ended up overriding this anyway**

After installing virtio drivers on the choose where to install windows installation page:

1. Click create parition so windows can automatically create the 3 expected partitions.
2. ```Shift + F10``` to open cmd
3. ```diskapart```
4. ```list disk```
5. ```select disk X```
~~6. ```create partition primary```~~
~~7. ```format fs=ntfs unit=65536 quick```~~
6. ```list partition```
7. ```exit``` x2

**If error in MBR**
in diskpart after selecting disk:
1. ```clean```
2. ```convert gpt```

## Snapshots 

### Create 

```lvcreate --snapshot --name snapshotname /data/windows```

### Revert 

Ensure windows vm stopped ```vm destroy /data/windows```

```lvchange --merge /data/snapshotname```

**Recreate snapshot to maintain point in time**