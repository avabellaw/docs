# Share folder using samba

* ```smbpasswd -a username``` add user and set sambda password - can be same username as linux user

## config

* ```public=yes``` no auth required
* ```guest ok``` Replaced by ```public=yes```
  * ```guest only``` guest even if auth
* ```browsable = yes``` Determines whether you will see automatically under network etc
* ```valid user = username``` Only this **samba** user can auth

works for vm

1. Edit samba conf
2. [name]
3. available = yes, etc
4. is SELINUX create folder outside home dir or set ```sudo setsebool -P samba_enable_home_dirs on```

## [Windows vm](/docs/linux-system/virtual-machines/windows-fedora.md)