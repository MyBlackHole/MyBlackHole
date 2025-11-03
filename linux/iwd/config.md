# config

- 配置固定 ip
```shell
sudo nvim /var/lib/iwd/BH.psk
[IPv4]
Address=192.168.1.100
Netmask=255.255.255.0
Gateway=192.168.1.1
Broadcast=192.168.1.255
DNS=192.168.1.1
```

- 关闭随机 mac
```shell
<!-- [General] -->
<!-- AddressRandomization=false -->


[Scan]
DisableMacAddressRandomization=true
```

- 开启 mac 随机化
```shell
[General]
AddressRandomization=network
AddressRandomization=true
```

- 第一次启动随机 mac
```shell
[General]
AddressRandomization=once__
```

- 不隐藏 mac 地址
```shell
[General]
AddressRandomization=disabled
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

- 开启 DHCP
```shell
[General]
EnableNetworkConfiguration=true
```

- 配置 DNS 后端
```shell
[Network]
NameReesolvingService=systemd
```

- 配置 PEAP 认证
```shell
sudo nvim /var/lib/iwd/BH.8021x

[Security]
EAP-Method=PEAP
EAP-PEAP-Phase2-Method=MSCHAPV2
EAP-PEAP-Phase2-Identity=dinggao.wu
EAP-PEAP-Phase2-Password=3edc#EDC
EAP-PEAP-Phase1-Phase2-AllowWithoutServerCert=true
EAP-PEAP-Phase1-AcceptAnyServerCert=true


[Settings]
AutoConnect=true

```
