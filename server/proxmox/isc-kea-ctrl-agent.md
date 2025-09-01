## Control from command line

### Release an ip 

curl -u [USERNAME]:[PASSWORD] -X POST -H "Content-Type: application/json"      -d '{
       "command": "lease4-del",
       "service": ["dhcp4"],"arguments": {
         "ip-address": "192.168.1.103"
       }
     }'      http://localhost:8000/