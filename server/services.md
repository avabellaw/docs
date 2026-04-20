## Jellyfin

Stop attempts to log to host log file

```systemctl mask systemd-journald-audit.socket```

### Edit log level

```/etc/jellyfin/logging.json```