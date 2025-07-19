# Add printer to CUPS

## Install cups

sudo apt install cups

### Configure acess to web admin

sudo nano /etc/cups/cupsd.conf

Edit the LISTEN ip address to the one your computer is on unless your connecting locally

## Remote server 

#create ssh tunnel: sudo ssh -D [any open port eg 9898] username@ip

#configure browser to use socks proxy with that port and the ip

add your user to lpadmin group
sudo usermod -a -G lpadmin [username]

ssh onto remote server and run the following command to allow remote connection:

cupsctl --remote-admin --remote-any --share-printers

## Admin

go to [localhost or remote ip]:631

for my hp printer:

1. Add new printer

2. Choose add as "ipp"

3. url is ipp://[ipaddress]/ipp/print


## Troubleshoot

* Sometimes just restart the printer.
