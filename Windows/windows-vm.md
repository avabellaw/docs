Installing using virtio for disk and using [**virtio win iso files**](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.285-1/) will assist in getting **clipboard and file sharing working**

## Install

[This video assists with install and optimisations](https://www.youtube.com/watch?v=q8ZsO-h14Po)

### Configurations

Set network card to use virtio (load driver when installing)

Create disk as virtio disk with writeback cache (load driver when installing)

use lscpu to work out how many threads your host computer has and set cpu accordingly (manually).

### Steps

load the **virtio win iso files** as an iso file.
Click load drivers and add network and disk drivers.

after installing, open 

## Optimisations

Disable "SysMain" service