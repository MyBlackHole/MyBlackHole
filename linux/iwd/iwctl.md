# iwctl

- connect
```shell
iwctl

device list
wanl0

device wanl0 set-property Powered on
adapter adapter set-property Powered on
station wanl0 scan
station wanl0 get-networks
station wanl0 connect SSID
```

- config
```shell
<!-- [General] -->
<!-- AddressRandomization=false -->


[Scan]
DisableMacAddressRandomization=true
```

- 固定 mac
```shell
sudo lvim /etc/iwd/main.conf (加入)

[General]
AddressRandomization=network

sudo lvim /var/lib/iwd/BH.psk (加入)
[Settings]
AddressOverride=0a:64:9b:b2:fe:b6
```

