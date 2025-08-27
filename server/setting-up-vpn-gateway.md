https://www.baeldung.com/linux/openvpn-forward-traffic-vpn-tunnel-iptables


iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE