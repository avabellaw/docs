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

## Wont umount

try lazy umount 

```umount -l [mountpoint]```

find process using mount 

```lsof +f -- [mountpoint]``` (-- just terminates lsof looking for parameters)