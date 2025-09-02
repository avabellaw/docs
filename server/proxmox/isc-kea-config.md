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
      "thread-pool-size": 4,
      "packet-queue-size": 28
    },
    "cache-threshold": 0.25,
    "calculate-tee-times": true,
    "valid-lifetime": 28800,
    "option-data": [
      {
        "name": "domain-name-servers",
        "data": "192.168.40.42, 192.168.40.82"
      }
    ],
    "hooks-libraries": [
      { "library": "/usr/lib/x86_64-linux-gnu/kea/hooks/libdhcp_stat_cmds.so" },
      { "library": "/usr/lib/x86_64-linux-gnu/kea/hooks/libdhcp_subnet_cmds.so" },
      {
        "library": "/usr/lib/x86_64-linux-gnu/kea/hooks/libdhcp_lease_cmds.so"
      },
      {
        "library": "/usr/lib/x86_64-linux-gnu/kea/hooks/libdhcp_ha.so",
        "parameters": {
          "high-availability": [
            {
              "this-server-name": "isc-kea",
              "mode": "hot-standby",
              "peers": [
                {
                  "name": "isc-kea",
                  "url": "http://192.0.0.1:8000/",
                  "role": "primary"
                },
                {
                  "name": "isc-kea-standby",
                  "url": "http://192.0.0.2:8000/",
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
        "subnet": "192.0.0.0/24",
        "id": 1,
        "option-data": [
          {
            "name": "routers",
            "data": "192.0.0.254"
          }
        ],
        "pools": [
          {
            "pool": "192.0.0.3-192.0.0.253"
          }
        ]
      }
    ],
  },
  "Control-agent": {
    "http-host": "127.0.0.1",
    "http-port": 8000
  },
  "loggers": [
        {
        "name": "kea-dhcp4",
        "severity": "INFO",
        "output_options": [
          {
            "output": "stdout"
          }
        ]
      },
      {
        "name": "lease-cmds-hooks",
        "severity": "INFO",
        "output_options": [
          { "output": "/var/log/kea/lease_cmds.log", "maxver": 5 }
        ]
      },
      {
        "name": "stat-cmds-hooks",
        "severity": "INFO",
        "output_options": [
          { "output": "/var/log/kea/stat_cmds.log", "maxver": 5 }
        ]
      },
      {
        "name": "high-availability-hooks",
        "severity": "INFO",
        "output_options": [
          { "output": "/var/log/kea/high-availability.log", "maxver": 5 }
        ]
      }
    ],

}