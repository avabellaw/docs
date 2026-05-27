## Clear out old fingerpints

clear root and user folder and only enroll for user.

```
sudo rm -rf /var/lib/fprint/$USER/
sudo rm -rf /var/lib/fprint/root/
```

sudo authselect enable-feature with-fingerprint
sudo authselect apply-changes

### Hardware reset

```
sudo dnf install usbutils
sudo usbreset /dev/bus/usb/003/003
```

## Enroll

enroll for specific user without sudo. You will need to "unlock" settings to give root permissions in order to access fingerprint settings in Gnome GUI.

```fprintd-enroll $USER```

Add/modify /etc/pam.d/system-auth:  
**Authselect will override these values if run**

```
auth        sufficient                                   pam_fprintd.so max-tries=5
auth        sufficient                                   pam_unix.so try_first_pass likeauth nullok
```

```sufficient```: the same as ```[success=done default=ignore]```

```try_first_pass```: Check if already authenticated in session

```likeauth```: If authenticated using other method (fingerprint) then auth.

```nullok```: Permits users to authenticate even if their account has an intentionally blank password.