User is btrbk_server

# Disable password auth for ssh

sudo nano /etc/ssh/sshd_config.d/01-local.conf

```
PasswordAuthentication no
```

# Has to be able to run btrfs without password

Add this to sudoers. Create new file in /etc/sudoers.d.

```
Cmnd_Alias BTRFS_FILESYSTEM_USAGE = /usr/bin/btrfs filesystem usage *
Cmnd_Alias BTRFS_FILESYSTEM_LIST = /usr/bin/btrfs filesystem list *
Cmnd_Alias BTRFS_SUBVOLUME_SHOW = /usr/bin/btrfs subvolume show *
Cmnd_Alias BTRFS_SUBVOLUME_LIST = /usr/bin/btrfs subvolume list *
Cmnd_Alias BTRFS_SUBVOLUME_SNAP = /usr/bin/btrfs subvolume snapshot *
Cmnd_Alias BTRFS_SUBVOLUME_DELETE = /usr/bin/btrfs subvolume delete *
Cmnd_Alias BTRFS_SEND = /usr/bin/btrfs send *
Cmnd_Alias BTRFS_RECEIVE = /usr/bin/btrfs receive *
Cmnd_Alias READLINK = /usr/bin/readlink *
Cmnd_Alias TEST = /usr/bin/test *

[user] ALL= NOPASSWD: BTRFS_FILESYSTEM_USAGE, BTRFS_FILESYSTEM_LIST, BTRFS_SUBVOLUME_SHOW, BTRFS_SUBVOLUME_LIST, BTRFS_SUBVOLUME_SNAP, BTRFS_SUBVOLUME_DELETE, BTRFS_SEND, BTRFS_RECEIVE, READLINK, TEST
```