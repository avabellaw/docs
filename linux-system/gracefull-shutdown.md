# Graceful shutdown

Shutdown using REISUB (Raise up - Reboot Even If System Utterly Broken - Raising Elephants Is Utterly Boring).

https://forum.manjaro.org/t/howto-reboot-turn-off-your-frozen-computer-reisub-reisuo/3855


REISUB (also known as the magic SysRq key ) is a mnemonic for:

    Rise up (from the dead) if you’re inclined to Zombie movies, like me … :zombie:
    R eboot E ven I f S ystem U tterly B roken
    R aising E lephants I s S o U tterly B oring

and is a gentle way of rebooting your system by doing the following:

    Switch the keyboard from R aw mode, used by programs such as X11 and SVGALib, to XLATE (translate) mode
    Send an E nd signal (SIGTERM) to all processes, except the boot process, allowing all processes to end gracefully
    Send an I nstant kill (SIGKILL) to all processes, except the boot process, forcing all processes to end .
    S ync all mounted filesystems, allowing them to write all data to disk.
    U nmount and remount all mounted filesystems in read-only mode.
    Re B oot the system


## Steps

Alt + "SysReq"/"prt scr"

**keep holding alt""

R E I S U B

## Ensure sysprint key is setup 

"The magic SysRq key is enabled when ```cat /proc/sys/kernel/sysrq``` shows 1."

"```sudo sysctl kernel.sysrq=1``` for the current session"

Make permanent:

```echo kernel.sysrq=1 | sudo tee --append /etc/sysctl.d/99-sysctl.conf```

**Alternatively** 

"set sysrq_always_enabled=1 as a kernel parameter to make the setting persist after a reboot.

The PrtSc is generally also the SysRq key. Just use Alt with PrtSc as normal. You can also try using AltGr in place of Alt."


https://unix.stackexchange.com/questions/668856/how-to-find-out-which-key-is-the-sysrq



