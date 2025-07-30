# Known issues

## Disable thermald

Thermald is intel cpu software designed to control cpu throttling etc in response to temperature.

Incompatible with my laptop 

Stop and disable the service for kernel to handle.

## Kernelstub option "quiet" causes reboot after poweroff

For some reason, the kernelstub option reboots the laptop as soon as you shut it down. (Unless using systemctl to poweroff).

Remove it using kernelstub -d "quiet".