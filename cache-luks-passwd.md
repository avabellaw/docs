# Unlock luks using cached passwd

## Cache the password after entering in LUKS

1. Ensure keyutils is installed
	* sudo apt install keyutils
2. Sudo nano /etc/crypttab
	* Find the encrypted volume line (eg cryptdata) and add 	following keyscript option:
	luks,keyscript=decrypt_keyctl
	
	cryptdata  UUID=<UUID>  none  luks,keyscript=decrypt_keyctl
	
	**This tells initramfs to cache the LUKS passphrase using the 						script**
3. Tell PAM to use the cached passwd
	a. sudo nano /etc/pam.d/common-password
	b. Add "use_authtok"
	password optional pam_gnome_keyring.so use_authtok
4. sudo update-initramfs -k all -c



