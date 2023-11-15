# frp

内网穿透
[github](git clone git@github.com:fatedier/frp.git)

![[imgs/Pasted image 20231114121702.png]]
## start
```shell
<!-- /etc/systemd/system/frps.service -->
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/frp/frps -c /usr/local/frp/frps.toml

[Install]
WantedBy=multi-user.target


<!-- ./frps -c ./frps.toml -->
tar zxvf frp_0.52.3_linux_amd64.tar.gz -C /usr/local/
mv /usr/local/frp_0.52.3_linux_amd64 /usr/local/frp

systemctl daemon-reload
systemctl start frps

systemctl status frps
● frps.service - Frp Server Service
   Loaded: loaded (/etc/systemd/system/frps.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2023-11-13 09:40:10 CST; 22s ago
 Main PID: 8721 (frps)
    Tasks: 6
   Memory: 13.5M
   CGroup: /system.slice/frps.service
           └─8721 /usr/local/frp/frps -c /usr/local/frp/frps.toml

Nov 13 09:40:10 node1 systemd[1]: Started Frp Server Service.
Nov 13 09:40:10 node1 frps[8721]: 2023/11/13 09:40:10 [I] [root.go:102] frps uses config file: /usr/local/frp/frps.toml
Nov 13 09:40:11 node1 frps[8721]: 2023/11/13 09:40:11 [I] [service.go:200] frps tcp listen on 0.0.0.0:7000
Nov 13 09:40:11 node1 frps[8721]: 2023/11/13 09:40:11 [I] [root.go:111] frps started successfully

systemctl enable frps



<!-- <!-- client --> -->
<!-- /etc/systemd/system/frpc.service -->
[Unit]
Description=Frp client Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.toml

[Install]
WantedBy=multi-user.target


systemctl daemon-reload
systemctl start frpc
systemctl enable frpc

ssh -oPort=2222 black@127.0.0.1
```
