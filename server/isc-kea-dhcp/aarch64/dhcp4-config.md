{
  "Dhcp4": {
    "control-socket": {
      "socket-type": "unix",
      "socket-name": "/var/run/kea/kea-dhcp4-socket"
    },
    "interfaces-config": {
      "interfaces": [
        "eth0"
      ]
    },
    "lease-database": {
      "type": "memfile",
      "persist": true,
      "name": "/var/lib/kea/kea-dhcp4-leases.csv"
    },
    "multi-threading": {
      "enable-multi-threading": true,
      "thread-pool-size": 0, // Auto detect
      "packet-queue-size": 28
    },
    "cache-threshold": 0.25,
    "calculate-tee-times": true,
    "valid-lifetime": 43200,
    "host-reservation-identifiers": [ "hw-address", "client-id" ],
    "option-data": [
      {
        "name": "domain-name-servers",
        "data": "192.168.1.25, 192.168.1.221, 1.1.1.1"
      }
    ],
    "hooks-libraries": [
      { "library": "/usr/lib/aarch64-linux-gnu/kea/hooks/libdhcp_host_cmds.so" },
      { "library": "/usr/lib/aarch64-linux-gnu/kea/hooks/libdhcp_stat_cmds.so" },
      {
        "library": "/usr/lib/aarch64-linux-gnu/kea/hooks/libdhcp_lease_cmds.so"
      },
      {
        "library": "/usr/lib/aarch64-linux-gnu/kea/hooks/libdhcp_subnet_cmds.so"
      },
      {
        "library": "/usr/lib/aarch64-linux-gnu/kea/hooks/libdhcp_ha.so",
        "parameters": {
          "high-availability": [
            {
              "this-server-name": "isc-kea-standby",
              "mode": "hot-standby",
              "max-unacked-clients": 0,
              "max-ack-delay": 10000,
              "heartbeat-delay": 7000,
              "max-response-delay": 40000,
              "multi-threading": {
                "enable-multi-threading": true,
                "http-client-threads": 0,
                "http-dedicated-listener": true,
                "http-listener-threads": 0
              },
              "peers": [
                {
                  "auto-failover": true,
                  "name": "isc-kea",
                  "url": "http://192.168.1.1:8001/",
                  "role": "primary"
                },
                {
                  "auto-failover": true,
                  "name": "isc-kea-standby",
                  "url": "http://192.168.1.221:8001/",
                  "role": "standby"
                }
              ]
            }
          ]
        }
      }
    ],
    "subnet4": [
      {
        "subnet": "192.168.1.0/24",
        "id": 1,
        "option-data": [
          {
            "name": "routers",
            "data": "192.168.1.254"
          }
        ],
        "pools": [
          {
            "pool": "192.168.1.3-192.168.1.220"
          }
        ]
      }
    ],
     "loggers": [
      {
        "name": "kea-dhcp4",
        "severity": "INFO",
        "output-options": [
          {
            "flush": true,
            "maxsize": 10240000,
            "maxver": 10,
            "output": "/var/log/kea/dhcp4.log",
          }
        ]
      },
      {
        "name": "kea-dhcp4.dhcpsrv",
        "severity": "INFO",
        "output-options": [
          {
            "flush": true,
            "maxsize": 10240000,
            "maxver": 10,
            "output": "/var/log/kea/dhcp4-dhcpsrv.log",
          }
        ]
      },
      {
        "name": "kea-dhcp4.leases",
        "severity": "INFO",
        "output-options": [
          {
            "flush": true,
            "maxsize": 10240000,
            "maxver": 10,
            "output": "/var/log/kea/dhcp4-leases.log",
          }
        ]
      }
    ]
  }
}