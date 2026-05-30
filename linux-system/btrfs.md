## Resize

Can set the new size or reduce/extend by:

```sudo btrfs filesystem resize [ +-]400G(GiB) /```

```sudo btrfs filesystem max /```

## quotas and qgroups

Enable Quotas: sudo btrfs quota enable <path>
Disable Quotas: sudo btrfs quota disable <path>

2. Setting Limits
Quotas are managed by "qgroups" (quota groups), which form a hierarchy. 

    Set Limit: sudo btrfs qgroup limit <size> <path> (e.g., sudo btrfs qgroup limit 20G /mnt/my-subvol)
    Remove Limit: Set the size to none
    Setting Limits on Subvolumes: Limits can be set on existing subvolumes or even before they are created. 

3. Viewing Quota Information

    Show Detailed Usage: sudo btrfs qgroup show -pcre <path>
    Raw Output (Bytes): sudo btrfs qgroup show --raw <path> 


btrfs filesystem du -s <path>. 

### Trigger rescan (full accounting only / not simple quota)

Manually trigger rescan to account for existing data. ```btrfs quota rescan /mnt```
* **-s** View progress of rescan
* **-w** Wait until finished

### Simple quotas 

1. ```btrfs quota disable /mnt```
2. ```btrfs quota enable --simple /mnt```
3. Check using simple quota not full accounting:```btrfs inspect-internal dump-super /dev/sdbX | grep SIMPLE_QUOTA```

## Wont umount

try lazy umount 

```umount -l [mountpoint]```

find process using mount 

```lsof +f -- [mountpoint]``` (-- just terminates lsof looking for parameters)

## No data copy-on-write (CoW)

Must be done on the directory level to be inherited by it's children.

This can be done with ```chattr +C /mnt/subvol```.

Only new files affected ofcourse.
