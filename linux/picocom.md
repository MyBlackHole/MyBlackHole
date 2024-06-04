# picocom

**串口调试工具**

## 安装
```shell
sudo apt-get install picocom 
```
## 快捷键
```
Ctrl+a进入转义模式，然后Ctrl+h 可以获取当前版本支持的命令 
Ctrl-u ：提高波特率 
Ctrl-d ：降低波特率 
Ctrl-f ：切换流控设置（硬件流控 RTS/CTS, 软件流控 XON/XOFF, 无 none） 
Ctrl-y ：切换奇偶校验 （偶 even, 奇 odd, 无 none） 
Ctrl-b : 切换数据位 （5, 6, 7, 8） 
Ctrl-c ：切换本地回显（local-echo）开关 
Ctrl-v ：显示当前串口参数和状态 
```

### 例子
| 命令                    | 说明                           |
| -------------------- | --------------------------- |
| picocom -b 115200 /dev/ttyUSB0  |进入Picocom终端模式 |
