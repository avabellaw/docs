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

1. Open config file
   For **system-wide** edit: ```/etc/xdg/autostart/gnome-keyring-ssh.desktop```        
   For **User specific** edit: ```~/.config/autostart/gnome-keyring-ssh.desktop```
2. Comment out ```Exec=/usr/bin/gnome-keyring-daemon --start --components=ssh```. This alone should work. If you dont want to do this, follow the rest of the steps but they haven't worked for me (Fedora).
3. Add to the config file:
    ```
    X-GNOME-Autostart-enabled=false
    Hidden=true
    ```
    <details>
    <summary>Script to add for system</details>
    ```
    echo $'X-GNOME-Autostart-enabled=false\nHidden=true' | sudo tee -a /etc/xdg/autostart/gnome-keyring-ssh.desktop
    ```
    </details>

    <details>
    <summary>Script to add for user</details>
    ```
    echo $'X-GNOME-Autostart-enabled=false\nHidden=true' >> ~/.config/autostart/gnome-keyring-ssh.desktop
    ```
    </details>

4. Ensure that ```Exec=/usr/bin/gnome-keyring-daemon --start --components=ssh``` is on the last line. 

### Test ssh agent

_The following will list available keys from the gnome keyring but not bitwarden_

```bash
ssh-add -l
```