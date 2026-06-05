# Setup considerations for vms (using KVM and queme)

## Storage

### RAW

Using raw is faster but loses storage efficiency (no dynamic sizing) and CoW protections.

On LVM-thin, you can overcommit storage.

### qcow2 

* Very flexible, minor performance implications due to overhead.
* Minor increase in CPU usage.

#### RAW on LVM-thin
 
* Faster than qcow2
* lvm-thin snapshots
  * No native snapshotting in virt-manager
  * LVM snapshots are v fast and efficient
* No fragmentation problems
* lowest CPU overhead and highest I/O throughput.


#### qcow2 on LVM (therfore w/ a fs)

* Faster without the having to disable CoW on BTRFS
* Requires lvm 

"Every single time your VM wants to save one piece of data, your computer has to do triple the work:
* The VM OS calculates where to write inside the guest.
* The QEMU process translates that write to find the right coordinate inside the .qcow2 file.
* The host filesystem looks at the .qcow2 file request and translates it to physical disk blocks.
* The LVM layer routes it to the physical drive hardware."

qcow
First Layer (qcow2): As the VM writes data, the qcow2 file allocates space in small chunks (clusters). These clusters get scattered randomly inside the virtual file as it grows.Second Layer (Host Filesystem): The host filesystem (like ext4 or XFS) takes that growing qcow2 file and breaks it up into pieces across the physical drive sectors.

#### raw on btrfs

* Takes up all space committed

#### qcow2 on btrfs

* Double CoW, so CoW needs to be disabled to avoid massive fragmentation.   