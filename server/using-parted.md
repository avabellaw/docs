## Shrink filesystem and partition 

[View the following on reddit - 2nd post](https://serverfault.com/questions/796758/shrink-partition-to-exactly-fit-the-underlying-filesystem-size)

Here is an example of the whole process.

This is the hard disk:

root@debian:~# fdisk /dev/loop0 -l

Disk /dev/loop0: 4,9 GiB, 5243928576 bytes, 10242048 sectors

Device       Boot Start     End Sectors  Size Id Type
/dev/loop0p1       8192 9765625 9757434  4,7G 83 Linux

Note: "/dev/loop0" is a loopback device, a virtual hard disk based on a file container. A physical hard disk would be named "/dev/sda".

The disk contains a single partition (/dev/loop0p1) that is 4.7GB in size.

This is the filesystem on the partition:

root@debian:~# df -h
/dev/loop0p1    4,7G    2,1G     0  45% /mnt

By default, it has the same size as the partition (4.7GB), but only 2.1GB (45%) of it is used. We could shrink the filesystem and partition to 2.1 GB without losing any data.

The first step is to use resize2fs with the -M switch on the partition. This command will attempt to move all files to the beginning of the filesystem to form one contiguous block, and then shrink it to the smallest possible size with no free space remaining. resize2fs only works with ext filesystems, for others you would have to find an equivalent program with this functionality.

root@debian:~# resize2fs -M /dev/loop0p1

The file system looks now like this:

root@debian:~# df -h
/dev/loop0p1    2,1G    2,1G     0  100% /mnt

The partition contains a 2.1GB filesystem that is 100% used. The rest of the partition is free space. However, the partition size is still 4.7GB.

To shrink the partition, the first step is to calculate the size of the filesystem in it. The dumpe2fs tool is very useful for that, it shows detailed information about a file system.

root@debian:~# dumpe2fs -h /dev/loop0p1 | grep '^Block*'
dumpe2fs 1.43.4 (31-Jan-2017)

Block count:              565950
Block size:               4096

This tells us that there are 565950 blocks and the block size is 4096 bytes. This allows us to calculate the filesystem size:

565950 blocks * 4096 bytes = 2318131200 bytes

From that, we can calculate the filesystem size in sectors. Let's have a look at the fdisk output again:

root@debian:~# fdisk /dev/loop0 -l

Disk /dev/loop0: 4,9 GiB, 5243928576 bytes, 10242048 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

Device       Boot Start     End Sectors  Size Id Type
/dev/loop0p1       8192 9765625 9757434  4,7G 83 Linux

This tells us that the hard disk sector size is 512 bytes, so:

2318131200 bytes / 512 bytes sector size = 4527600 sectors

Because the partition does not start at sector 0, we need to add the start sector (8192) from the fdisk output above, then subtract one because the sector count is zero-based:

4527600 + 8192 (start sector) - 1 = 4535791

This is the new end sector for our partition.

Now we use parted to shrink the partition. "unit s" switches the display unit to sectors, and "resizepart [partnumber] [newsize]" shrinks the partition to the smaller size in sectors that we calculated above.

root@debian:~# parted /dev/loop0
GNU Parted 3.2
                   
Using /dev/loop0

(parted) unit s                                                           
(parted) p

Model: Loopback device (loopback)
Disk /dev/loop0: 10242048s
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start  End       Size      Type     File system  Flags
 1      8192s  9765625s  9757434s  primary  ext4

(parted) resizepart 1 4535791  
                                       

It gives us a warning about potential data loss, but there should be no data in the sectors to be cut off. We now have a 2.1GB partition with a 2.1GB filesystem that's 100% used.
