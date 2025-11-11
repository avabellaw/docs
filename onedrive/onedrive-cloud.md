---
parent: OneDrive
---

# rclone bug

Older versions such as v1.60.1 will only use readonly permissions. Debian like proxmox may only have the older version in apt.

# Live access to onedrive cloud

1. Install rclone

2. Use rclone config to add onedrive using defaults

3. Create a Onedrive-Cloud dir to mount to 

3. rclone mount [label used in setup eg onedrive-cloud]: ~/Onedrive-Cloud \
--vfs-cache-mode full \
--vfs-cache-max-age ... \
--vfs-cache-max-size ... (10G)
