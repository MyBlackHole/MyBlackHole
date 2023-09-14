# ap

连 wifi 同时开热点



```shell
sudo apt install util-linux bash procps hostapd iproute2 iw haveged git
git clone https://github.com/oblique/create_ap
cd create_ap
<!-- sudo make install -->


 cd /path/to/create_ap/
 sudo su  # 在root下运行

 nohup ./create_ap wlp3s0 lo BH 1358244533 &
```
