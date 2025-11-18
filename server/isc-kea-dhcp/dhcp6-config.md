{
  "Dhcp6": {
    "control-socket": {
      "socket-type": "unix",
      "socket-name": "/var/run/kea/kea-dhcp6-socket"
    },
    "interfaces-config": {
      "interfaces": [
        "eth0"
      ]
    },
    "multi-threading": {
      "enable-multi-threading": true,
      "thread-pool-size": 4,
      "packet-queue-size": 28
    },
    "option-data": [
      {
        "name": "dns-servers",
        "code": 23,
        "space": "dhcp6",
        "csv-format": true,
        "data": "2606:4700:4700::1111" #Add own dns
      }
    ],
    "loggers": [
      {
        "name": "kea-dhcp6",
        "severity": "INFO",
        "output_options": [
          {
            "output": "/var/log/kea/dhcp6.log",
            "maxver": 10
          }
        ]
      },
    ]
  }
}
