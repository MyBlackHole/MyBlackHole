# progress

## 安装
```shell
# ubuntu
sudo apt install progress

# centos
yum install progress
```

## 语法 
progress [-qdwmM] [-W secs] [-c command] [-p pid]

### 选项
-q / --quiet                 隐藏所有打印的消息。  
-w / --wait                 显示IO的吞吐量和剩余时间。
-m / --monitor            持续监控进程直到要监控进程的退出或者手动按 Ctrl+C 退出。
-a / --additional-command  添加命令到默认监控命令列表。
-c / --command  监控指定命令的名称 (ex: firefox)。
-p / --pid id               监控指定进程的 PID (ex: pidof firefox)。
-i / --ignore-file file        忽略指定文件。
-o / --open-mode {r|w}       报告文件的打开模式。
-v / --version                 打印命令的版本。
-h / --help                    打印帮助信息。

## 例子
- 要查看默认监控的命令列表
```shell
progress --help | head -n 6 | tail -n 1
```

- 查看 cp 命令复制进度
```shell
cp bigfile newfile & progress -mp $!
```

- 查看 tar 命令压缩和解压文件进度
```shell
tar czfv archive.tar.gz source & progress -mp $!
```

- 查看 mv 命令移动文件进度
```shell
mv source destination & progress -mp $!
```
