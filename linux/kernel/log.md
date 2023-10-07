# log

1. 内核引导日志目录
/var/log/dmesg

2. ssh登陆记录信息，包括失败的记录信息。
/var/log/secure

3. 记录邮件相关信息
/var/log/maillog

4. 记录crontab相关信息
/var/log/cron

5. 记录ftp相关的日志信息
/var/log/xferlog

6. 系统大部分日志，包括login、check password、failed log等
/var/log/messages

## loglevel

> 改变console loglevel的方法有如下几种：
1. 启动时Kernel boot option：loglevel=level
2. 运行时Runtime: dmesg -n level
（注意：demsg -n level 改变的是console上的loglevel，dmesg命令仍然会打印出所有级别的系统信息。）
3. 运行时Runtime: echo $level > /proc/sys/kernel/printk
4. 运行时Runtime:写程序使用syslog系统调用（可以man syslog）
