## Copy/paste

**Can't copy paste into vm console?**

1. Add a serial port, either command line or promox GUI

```qm set [your_vm_id] -serial0 socket```

2. Configure GRUB (sorry if not grub)

    - edit /etc/default/grub	
    - Replace the line **GRUB_CMDLINE_LINUX_DEFAULT=…** with
    ```GRUB_CMDLINE_LINUX_DEFAULT="quiet console=tty0 console=ttyS0,115200"	```
    - ```sudo update-grub```

Now use _Console > xterm.js_ instead of noVNC

### Credit

[silicon blog](https://silicon.blog/2023/01/12/how-to-enable-copy-and-paste-function-on-your-proxmox-web-console-without-install-additional-software-in-your-vm/)