## Create new 

```pvcreate /dev/sdX```
```vgcreate volgroup /dev/sdX```
```lvcreate -l [extends%] --name [lname] [volgroup]```
```         -L [sizeG]                             ```

## logical colume

-T |--thin thin lv

## Move all extents (data/vgs&lvs) to another drive

1. Pvcreate lvm onto 2nd drive.
2. Add the vg to the new 2nd drive pv.
3. pvmove [original pv] - will move to only other existing pv
4. vgreduce [vg] [original pv]