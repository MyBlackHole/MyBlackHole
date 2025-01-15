# printk

kern_levels

```shell
cat /proc/sys/kernel/printk
1       4       1       4

该文件有四个数字值，它们根据日志记录消息的重要性，定义将其发送到何处。
关于不同日志级别的更多信息，请查阅syslog(2)联机帮助。
上面显示的4个数据分别对应：

控制台日志级别：优先级高于该值的消息将被打印至控制台
默认的消息日志级别：将用该优先级来打印没有优先级的消息
最低的控制台日志级别：控制台日志级别可被设置的最小值(最高优先级)
默认的控制台日志级别：控制台日志级别的缺省值
数值越小，优先级越高

```

```shell
<!--include/linux/kern_levels.h-->

printk(日志級別 "消息文本")；
pr_info、pr_warn、pr_err 等等都是printk的宏，用来打印不同级别的日志。

日志級別一共有8個級別，printk 的日志級別定義如下：

#define KERN_EMERG	KERN_SOH "0"        /* 緊急事件消息，系統崩潰之前提示，表示系統不可用 */
#define KERN_ALERT	KERN_SOH "1"        /* 報告消息，表示必須立即采取措施 */
#define KERN_CRIT	KERN_SOH "2"        /* 臨界條件，通常涉及嚴重的硬件或軟件操作失敗 */
#define KERN_ERR	KERN_SOH "3"        /* 錯誤條件，驅動程序常用KERN_ERR來報告硬件的錯誤 */
#define KERN_WARNING	KERN_SOH "4"    /* 警告條件，對可能出現問題的情況進行警告 */
#define KERN_NOTICE	KERN_SOH "5"        /* 正常但又重要的條件，用於提醒。常用於與安全相關的消息 */
#define KERN_INFO	KERN_SOH "6"        /* 提示信息，如驅動程序啟動時，打印硬件信息 */
#define KERN_DEBUG	KERN_SOH "7"        /* 調試級別的消息 */
#define KERN_DEFAULT	""              /* 默認采用的級別 */

沒有指定日志級別的 printk 語句默認采用的級別是 DEFAULT_ MESSAGE_
```

- 配置启用 DEBUG 内核日志打印等级
```shell
cat /proc/sys/kernel/printk
make -C $(LINUX_KERNEL_PATH) M=$(CURRENT_PATH) KCFLAGS+=-DDEBUG modules
```


## loglevel

- 所有消息都打印到控制台
```shell
echo 8 > /proc/sys/kernel/printk: 
```

- 改变console loglevel的方法有如下几种：
```shell

1. 启动时Kernel boot option：loglevel=level

2. 运行时Runtime: dmesg -n level
（注意：demsg -n level 改变的是 console 上的 loglevel，dmesg 命令仍然会打印出所有级别的系统信息。）

3. echo $level > /proc/sys/kernel/printk

4. 写程序使用 syslog 系统调用（可以man syslog）

```
