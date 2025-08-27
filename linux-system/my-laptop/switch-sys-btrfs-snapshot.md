

    Used mv to rename the active root subvolume to something like @-root-borked.

    Used btrfs sub snapshot @-root-snapshot @ to create a new read write subvolume based off the read only snapshot I had taken before the upgrade in the place of the old root subvolume.

    Reboot
