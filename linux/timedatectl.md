# timedatectl

命令用于查看当前时区设置

![[imgs/timedatectl.png]]


- 时间同步
```shell
timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp true


timedatectl status
               Local time: Wed 2024-06-05 10:37:15 CST
           Universal time: Wed 2024-06-05 02:37:15 UTC
                 RTC time: Wed 2024-06-05 02:37:14
                Time zone: Asia/Shanghai (CST, +0800)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```
