# Install bitwarden 

**Use snap not the flatpak**

Flatpak is unable to use biometrics and ssh agent

snap has continous auto-updates. Unfortunately, fingerprint doesn't work for browser extension _does work on the actual desktop snap app_.

## SSH agent

**You will need to add a fix to stop SSH_AUTH_SOCK from being override.**

Just edit this file for fedora. ~~It will disable gcr (gnome keyring) from overriding the SSH_AUTH_SOCK var~~ supposed to but have to do the below as well:

nano ~/.ssh/config

```                                                           
Host *
    IdentityAgent /home/ava/snap/bitwarden/current/.bitwarden-ssh-agent.sock

```

Private keys in bitwarden, public in home dir .ssh.

For reference [bitwarden website: changing ssh agent to point to Bitwarden](https://bitwarden.com/help/ssh-agent/#tab-linux-6VN1DmoAVFvm7ZWD95curS)

### Disable gnome ssh agent to prevent overriding 

#### System-wide

```
echo $'X-GNOME-Autostart-enabled=false\nHidden=true' | sudo tee -a /etc/xdg/autostart/gnome-keyring-ssh.desktop
```

#### User specific

```
echo $'X-GNOME-Autostart-enabled=false\nHidden=true' >> ~/.config/autostart/gnome-keyring-ssh.desktop
```

### Test ssh agent

_The following will list available keys_

```bash
ssh-add -l
```