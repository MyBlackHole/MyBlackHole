# nethogs

进程流量监控工具

## install
```shell
sudo apt-get install nethogs
paru -S nethogs
```

## 参数
```shell
用法：nethogs[-v][-h][-b][-d秒][-v模式][-c计数][-t][-p][-s][设备[设备…]]
-V：打印版本。
-H：打印这个帮助。
-B：BugHunt模式-表示跟踪模式。
-D：更新刷新率的延迟（秒）。默认值为1。
-V：查看模式（0=kb/s，1=total kb，2=total b，3=total mb）。默认值为0。
-C：更新次数。默认值为0（无限制）。
-TraceMod。
-P：在混乱模式下嗅探（不推荐）。
-S：按发送列对输出进行排序。
-A：监控所有设备，甚至是回送/停止的设备。
设备：要监视的设备。默认值是除环回之外的所有已启动和正在运行的接口

NetHogs运行时，按：
q：退出
S：按发送流量排序
R：按接收流量排序
M：在总计（kb、b、m b）和kb/s模式之间切换
```
