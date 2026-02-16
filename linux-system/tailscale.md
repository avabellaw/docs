Key details regarding --shields-up:

    Purpose: It is designed to secure a device by rejecting all incoming traffic from other machines in your tailnet.
    Outgoing Traffic: The node can still initiate connections to other devices in the tailnet.
    Use Case: Ideal for mobile devices or servers that need to access services but should not be accessible themselves, acting as an extra layer of protection.
    Behavior: When enabled, it rejects traffic at the packet filter level.
    Disabling: To turn it off, run tailscale up --shields-up=false. 

## Allow tailscale on nordvpn 

```nordvpn whitelist add port 41641```

## Firewall block every connection but tailscale

https://tailscale.com/docs/how-to/secure-ubuntu-server-with-ufw