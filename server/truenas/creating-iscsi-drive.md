https://www.truenas.com/docs/core/13.0/coretutorials/sharing/iscsi/usingiscsishare/
https://dustymabe.com/2024/05/10/configuring-and-using-iscsi/

Create zvol
Create iscsi mount 

sudo iscsiadm --mode discovery --type sendtargets --portal 127.0.0.1

Next, enter command sudo iscsiadm \--mode node \--targetname {BASENAME}:{TARGETNAME} \--portal {IPADDRESS} \--login, where {BASENAME} and {TARGETNAME} (without curly brackets) is the information from the discovery command.

#### Disconnect from drive

To view whats connected:
iscsiadm -m session 

iscsiadm -m node 192.168.1.67:3260 -u

or replace the ip with --targetname=[iqn*]

### Fedora install of iscsi

sudo dnf install -y targetcli iscsi-initiator-utils
sudo systemctl enable target
sudo reboot