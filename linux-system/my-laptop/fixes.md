# Known issues

## Disable thermald

Thermald is intel cpu software designed to control cpu throttling etc in response to temperature.

Incompatible with my laptop 

Stop and disable the service for kernel to handle.

### Improve battery life with alternative power manager

```sudo apt install tlp tlp-rdw```
```sudo systemctl enable tlp.service```

#### Add stop/start charge threshold

in ```/etc/tlp.conf```:

```
START_CHARGE_THRESH_BAT0=80
STOP_CHARGE_THRESH_BAT0=90
```

## Reboot after poweroff/hibernate

### Kernelstub option "quiet" causes reboot after poweroff

For some reason, the kernelstub option reboots the laptop as soon as you shut it down. (Unless using systemctl to poweroff).

Remove it using kernelstub -d "quiet".

### Disable USB wake - Didn't work

ava@ava-pop:~$ cat /proc/acpi/wakeup | grep -i xhc
XHCI	  S3	*enabled  pci:0000:00:14.0

**Disable USB boot with the following**
echo "XHCI" | sudo tee /proc/acpi/wakeup

### 
