# error

- 无法设置禁用随机 mac (NetworkManager 做前端)
```shell
sudo lvim /etc/NetworkManager/conf.d/wifi_rand_mac.conf
[device]
wifi.scan-rand-mac-address=no
```
