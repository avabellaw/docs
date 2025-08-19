## Setup repos, remove nag msg

Find community-scripts for Promox and run the Post Installation script.

## Set up locals

apt install locales -y
dpkg-reconfigure locales

 chose en_GB.utf-8 ofce


## Tailscale

1. Install tailscale using script from website in web ui shell.
2. Run ```tailscale up --ssh```
3. Click link to auth

### Tailscale certificate

1. Enable HTTPS certs in admin console
2. [Run tailscale script](https://tailscale.com/kb/1133/proxmox#troubleshooting)
3. Create a cronjob daily or similar, expires after 90 days