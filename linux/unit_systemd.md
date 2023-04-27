# systemctl
![[imgs/Pasted image 20230428011440.png]]
## Service unit：系统服务
### Unit
![[imgs/Pasted image 20230428010948.png]]
### Service
![[imgs/Pasted image 20230428011047.png]]
### install
![[imgs/Pasted image 20230428011120.png]]
### Target unit：多个 Unit 构成的一个组 

### Device Unit：硬件设备 

### Mount Unit：文件系统的挂载点 

### Automount Unit：自动挂载点 

### Path Unit：文件或路径 

### Scope Unit：不是由 Systemd 启动的外部进程 

### Slice Unit：进程组 

### Snapshot Unit：Systemd 快照，可以切回某个快照 

### Socket Unit：进程间通信的 socket 

### Swap Unit：swap 文件 

### Timer Unit：定时器
## 管理
```shell
# 立即启动一个服务 

$ sudo systemctl start apache.service 

# 立即停止一个服务 

$ sudo systemctl stop apache.service 

# 重启一个服务 

$ sudo systemctl restart apache.service 

# 杀死一个服务的所有子进程 

$ sudo systemctl kill apache.service 

# 重新加载一个服务的配置文件 

$ sudo systemctl reload apache.service 

# 重载所有修改过的配置文件 

$ sudo systemctl daemon-reload 

# 显示某个 Unit 的所有底层参数 

$ systemctl show httpd.service 

# 显示某个 Unit 的指定属性的值 

$ systemctl show -p CPUShares httpd.service 

# 设置某个 Unit 的指定属性 

$ sudo systemctl set-property httpd.service CPUShares=500R
```

## 例子
```ini
[Unit] 

Description=test 

Documentation=test 

After=network.target 

Wants=sshd-keygen.service 

[Service] 

ExecStart=/home/black/11111 

Type=simple 

KillMode=process 

Restart=on-failure 

RestartSec=42s 

[Install] 

WantedBy=multi-user.target
```