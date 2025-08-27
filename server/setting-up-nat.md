## Create another virtual bridge eg vmbr1

### Masquerade ips

Enable portforwarding.

MASQUERADE the ip addresses so that the sources ips look correct externally and internally.

```
auto vmbr1
iface vmbr1 inet static
        address 10.0.0.1/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0

        **post-up echo 1 >> /proc/sys/net/ipv4/ip_forward**
        **post-up iptables -t nat -A POSTROUTING -s '10.0.0.0/24' -o vmbr0 -j MASQUERADE**
        **post-down iptables -t nat -D POSTROUTING -s '10.0.0.0/24' -o vmbr0 -j MASQUERADE**
```

## Route host ports to the internal container ips

iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport [host port] -j DNAT --to [container ip]:[container port]

## allow internet access through the vpn and lan access through the vpns gateway.

Add ip route to container.

```nano /etc/network/interfaces```

```
auto eth0
iface eth0 inet static
        address 10.10.10.105/24
        gateway 10.10.10.2

auto lan0
iface lan0 inet static
        address 10.0.0.105/24

        # route to open lan (192.) through vpn gateway (10.)
        post-up ip route add 192.168.1.0/24 via 10.10.10.1 dev eth0
        post-down ip route del 192.168.1.0/24 via 10.10.10.1 dev eth0
```

## Netoworking ubunutu 24.04

stored here:

```/etc/systemd/network/```