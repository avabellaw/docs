---
parent: OneDrive
---

# Live access to onedrive cloud

1. Install rclone

2. Use rclone config to add onedrive using defaults

3. Create a Onedrive-Cloud dir to mount to 

3. rclone mount [label used in setup eg onedrive-cloud]: ~/Onedrive-Cloud \
--vfs-cache-mode full \
--vfs-cache-max-age ... \
--vfs-cache-max-size ... (10G)
