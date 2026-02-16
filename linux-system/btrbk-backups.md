## Backing up to external drive (automount)

Add nofail,x-systemd.automount to fstab to automount and not fail on no mount

x-systemd.idle-timeout=5min

Search for mount files after adding to fstab with 

```systemctl list-units --type=mount```

Find the systemd mnt identifier "media-[MOUNTNAME]".

service file automount:

```
[Unit]
Description=Trigger btrbk backup to external drive after mount
Requires=media-external.mount
After=media-external.mount

[Service]
ExecStart=

[Install]
WantedBy=media-external.mount
```

Add script or directly to ExecStart:
```btrbk archive [snapshot dir] [external drive]```