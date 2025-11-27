## Register new agent on server with isc-dhcp

```su stork-agent -s /bin/sh -c 'stork-agent register --server-url http://10.0.0.117:8080'```

## Control from command line

### Release an ip 

curl -u [USERNAME]:[PASSWORD] -X POST -H "Content-Type: application/json"      -d '{
       "command": "lease4-del",
       "service": ["dhcp4"],"arguments": {
         "ip-address": "192.168.1.103"
       }
     }'      http://localhost:8000/

### Reclaim ips

curl -u [user]:[pass] \
     -H "Content-Type: application/json" \
     -d '{ "command": "leases-reclaim", "service": [ "dhcp4" ], "arguments": { "remove": true } }' \
  http://127.0.0.1:8000/

}

## kea shell

### List all dhcp4 leases

kea-shell --auth-user [user] --auth-password [pass] lease4-get-all --service dhcp4


## Postgresql

### View leases

SELECT CAST(address AS text), CAST(hostname AS text), CAST(client_id AS text), expire FROM lease4;


## Ctrl agent config

```
{
    "Control-agent": {
        "http-host": "127.0.0.1",
        "http-port": 8000,
        "http-headers": [
            {
                "name": "Strict-Transport-Security",
                "value": "max-age=31536000"
             }
        ],
        "authentication": {
            "type": "basic",
            "realm": "kea-control-agent",
            "directory": "/etc/kea",
        "clients": [
            {
                "user": "kea-api",
                "password-file": "kea-api-password"
            }
        ]
        },

        "control-sockets": {
            "dhcp4": {
                "comment": "dhcp4 main server",
                "socket-type": "unix",
                "socket-name": "/var/run/kea/kea-dhcp4-socket"
            },
            "dhcp6": {
                "comment": "dhcp6 main server",
                "socket-type": "unix",
                "socket-name": "/var/run/kea/kea-dhcp6-socket"
            }

        },

        "loggers": [ {
            "name": "kea-ctrl-agent",
            "severity": "INFO"
        } ]
    }
}
```