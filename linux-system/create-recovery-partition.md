# Create a recovery partition **UNTESTED**

Flash a live image to a new partition so that it has non persistant changes.

1. Format the partition as FAT32, this will allow you to identify the drive unambigously.
   * It's easy and quick to do and reduces chance for error
2. Use dd to flash the iso image
   ```dd if=/path/to/iso of=/dev/sdX bs=4M```
   **bs=4M** Default block size is 512 bytes, setting to 4M is faster.
   **status=progress** Self explanitory, default is silent.
   **sync** Forces direct sync to disk instead of writing to RAM first
        Gives you accurate view of when finished, reduces chance of corruption.
        Alternatively, you could reduce to minimal risk and watch when writes finish
            1. Simply run ```sync``` after the dd command
            2. ```
               sudo dnf install sysstat
               iostat -xz 1
               ```
            3. Check for memory waiting to be written to disk (Watch every 2s) ```watch -n 2 "grep -e Dirty /proc/meminfo"```

## Make changes to image (Fedora)

**Livemedia-creator**

```sudo dnf install livecd-tools spin-kickstarts```

get the .ks base template from ```/usr/share/spin-kickstarts/```

* **Packages** Under ```%packages``` section, add your desired package names.
* **Scripts** Under ```%post```, add scripts.

**Create recovery image:**

```sudo livemedia-creator --ks custom-recovery.ks --no-virt --resultdir ./build-output --project "Custom Fedora Recovery" --make-iso```

Flash using dd (see above).

### Inject custom scripts

Add them directly to the %post section. create a dir, cat script content to a file in dir, make executable.

```
cat << 'EOF' > /root/new-dir-name/MY_CUSTOM_SCRIPT.sh
#!/bin/bash
echo "im a super useful script that echos... well... this"
EOF
```


