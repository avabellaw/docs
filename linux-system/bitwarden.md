# Install bitwarden 

**Use snap not the flatpak**

Flatpak is unable to use biometrics and ssh agent

snap has continous auto-updates. Unfortunately, fingerprint doesn't work for browser extension _does work on the actual desktop snap app_.

## SSH agent

**You will need to add a fix to stop SSH_AUTH_SOCK from being override.**

[Explained on StackOverflow](https://unix.stackexchange.com/questions/315004/where-does-gnome-keyring-set-ssh-auth-sock)

In /etc/profile.d/bitwarden-ssh, add the following:

```
export GSM_SKIP_SSH_AGENT_WORKAROUND=1

export SSH_AUTH_SOCK=/home/<user>/snap/bitwarden/current/.bitwarden-ssh-agent.sock
```

Private keys in bitwarden, public in home dir .ssh.

[Changing ssh agent to point to Bitwarden, found on Bitwarden website](https://bitwarden.com/help/ssh-agent/#tab-linux-6VN1DmoAVFvm7ZWD95curS)

### Test ssh agent

_The following will list available keys_

```bash
ssh-add -l
```