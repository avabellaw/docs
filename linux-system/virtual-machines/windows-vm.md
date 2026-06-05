# Windows VM setup

Now using raw disk on LVM-Thin for performance gains while maintaining ease of use. (Helps that it gives me a reason to keep LVM). [See considerations](../virtual-machine-considerations.md)

## Snapshots 

### Create 

```lvcreate --snapshot --name snapshotname /data/windows```

### Revert 

Ensure windows vm stopped ```vm destroy /data/windows```

```lvchange --merge /data/snapshotname```

**Recreate snapshot to maintain point in time**