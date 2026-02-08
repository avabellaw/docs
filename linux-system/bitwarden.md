# Install bitwarden 

**Use snap not the flatpak**

Flatpak is unable to use biometrics and ssh agent

## SSH agent

~~Add the following to /etc/environment in order to use bitwardens ssh agent system-wide.~~

Add to /etc/profile prefixed with 'export'

```SSH_AUTH_SOCK=/home/<user>/snap/bitwarden/current/.bitwarden-ssh-agent.sock```

Private keys in bitwarden, public in home dir .ssh.



### Test ssh agent

_The following will list available keys_

```bash
ssh-add -l
```