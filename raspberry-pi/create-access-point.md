# Create
nmcli connection add type wifi ifname wlan0 mode ap ssid "Raspi WiFi" con-name wifi-ap

# Secure it
nmcli connection modify wifi-ap 802-11-wireless-security.key-mgmt wpa-psk
nmcli connection modify wifi-ap 802-11-wireless-security.psk $wifipass

wifipass=""

# Share internet
nmcli connection modify wifi-ap ipv4.method shared

# (optional) prevent autoconnect on boot
#nmcli connection modify wifi-ap connection.autoconnect no

# Bring it up
nmcli connection up wifi-ap

