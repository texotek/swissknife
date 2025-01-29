# NetworkManager

## Mac Address Randomization

NetworkManager.conf
```
[device-mac-randomization]
    # "yes" is already the default for scanning
    wifi.scan-rand-mac-address=yes

    [connection-mac-randomization]
    ethernet.cloned-mac-address=random
    wifi.cloned-mac-address=random
```

## Prevent Hostname Leakage


To prevent leaking the hostname via dhcp: 

NetworkManager.conf
```
[ipv4]
    dhcp-send-hostname=false
    dhcp-hostname=

[ipv6]
    dhcp-send-hostname=false
    dhcp-hostname=
    ip6-privacy=2
```
