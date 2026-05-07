## System authentication (Fedora)

https://wiki.gentoo.org/wiki/YubiKey/PAM

[_More info fedora magazine_](https://fedoramagazine.org/use-fido-u2f-security-keys-with-fedora-linux/)

1. Install PAM modules and fido2-tools ```sudo dnf install pam-u2f pamu2fcfg fido2-tools```
2. Add new configuration using pam2fcfg
   ```
   mkdir -p ~/.config/Yubico
   pamu2fcfg [--pin-verification] > ~/.config/Yubico/u2f_keys
   ``` 
   (I don't think you need the --pin-verification argument _with my key atleast_)

   For backup key, use: ```pamu2fcfg [--nouser or -n] [--pin-verification] >> ~/.config/Yubico/u2f_keys```

Following is based on this detailed and very well written guide: https://wiki.gentoo.org/wiki/YubiKey/PAM

### System auth: GDM / sudo

/etc/pam.d/sudo contains system-auth config so just add there for both.

```sudo nano /etc/pam.d/system-auth```

```auth        sufficient                  pam_u2f.so      cue```

cue: Please touch yubikey
sufficient: Can use this or other methods

### Remove pin

edit the generated config and remove +pin ```~/.config/Yubico/u2f_keys```

