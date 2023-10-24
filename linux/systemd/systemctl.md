# systemctl

Systemd 的主命令，用于管理系统

[配置细节](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)

## 选项

 |   |   |
|---|---|
|sudo systemctl reboot|重启系统|
|sudo systemctl poweroff|关闭系统，切断电源|
|sudo systemctl halt|CPU停止工作|
|sudo systemctl suspend|暂停系统|
|sudo systemctl hibernate|让系统进入冬眠状态|
|sudo systemctl hybrid-sleep|让系统进入交互式休眠状态|
|sudo systemctl rescue|启动进入救援状态（单用户状态）|

## 例子

- 重启系统
$ sudo systemctl reboot

- 关闭系统，切断电源
$ sudo systemctl poweroff

- CPU停止工作
$ sudo systemctl halt

- 暂停系统
$ sudo systemctl suspend

- 让系统进入冬眠状态
$ sudo systemctl hibernate

- 让系统进入交互式休眠状态
$ sudo systemctl hybrid-sleep

- 启动进入救援状态（单用户状态）
$ sudo systemctl rescue

- 服务配置
```shell
[Unit]
Description=AIO Worker Service (storage)
After=network.target aio.netinit.service

[Service]
WorkingDirectory=/opt/aio/airflow/bin
ExecStartPre=/usr/bin/echo ok
ExecStartPre=/usr/bin/echo ok1
ExecStart=/usr/bin/echo ok3
ExecStart=/usr/bin/echo ok4
EnvironmentFile=/opt/aio/cfg/airflow.runtime.env
EnvironmentFile=/opt/aio/cfg/worker.env
Type=simple
Restart=on-failure
#KillSignal=SIGQUIT

[Install]
WantedBy=multi-user.target


<!-- out -->
ok
ok1
ok4
```
