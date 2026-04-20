## Save more power when idle 

Show current cpu scaling governor, for proxmox default is performance:

```cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor```

Show available scaling governers:

```cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors```

See frequency of every core:

```watch -n 1 "grep MHz /proc/cpuinfo"```

Change the scaling govener:

```echo "powersave|schedutil|performance|" | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor```

### Best for amd apci, proxmox

schedutil

powersave stuck at lowest freq.

### Set in bios 

__Set the scaling governer in GRUB__


Edit /etc/default/grub and add the following to GRUB_CMDLINE_LINUX_DEFAULT:
text

cpufreq.default_governor=schedutil



