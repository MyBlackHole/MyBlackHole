# dhcpcd

## config

```shell
sudo nvim /etc/dhcpcd.conf

interface wlan0
static ip_address=192.168.1.217/24
static routers=192.168.1.1
static domain_name_servers=8.8.8.8
```
