# iwctl

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
