[See Windows vm setup](/docs/linux-system/virtual-machines/windows-vm.md)

## Network share

### Samba

1. [Setup samba share](/docs/linux-system/share-folder-samba.md)
2. IP of the samba share is the hosts ip on that interface (the gateway of the NAT).
   
open turn on/off features: in run box: ```appwiz.cpl```

turn on smb features not just direct

map drive and choose credentials otherwise windows tries to use windows creds and fails slighty with 80004005

#### Firewall (Fedora - firewalld)

```firewalld-cmd --permanent --zones=libvirt --add-service=samba reload```

```--permanent``` Save to disk

```zones=libvirt``` Open samba to just the vms

```--add-service=samba``` Predefined ports to allow instead of manually adding them
