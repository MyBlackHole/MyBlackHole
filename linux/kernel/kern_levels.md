# Linux Kernel printk Levels

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

```c
int console_printk[4] = {
    DEFAULT_CONSOLE_LOGLEVEL,       /* console_loglevel */
    DEFAULT_MESSAGE_LOGLEVEL,       /* default_message_loglevel */
    MINIMUM_CONSOLE_LOGLEVEL,     /* minimum_console_loglevel */
    DEFAULT_CONSOLE_LOGLEVEL,       /* default_console_loglevel */
};

/*内核通过 printk() 输出的信息具有日志级别，*/
/*日志级别是通过在 printk() 输出的字符串前加一个带尖括号的整数来控制的，*/
/*如printk("<6>Hello, world!\n");。内核中共提供了八种不同的日志级别，在 linux/kernel.h 中有相应的宏对应。*/

#define KERN_SOH	"\001"		/* ASCII Start Of Header */
#define KERN_EMERG	KERN_SOH "0"	/* system is unusable */
#define KERN_ALERT	KERN_SOH "1"	/* action must be taken immediately */
#define KERN_CRIT	KERN_SOH "2"	/* critical conditions */
#define KERN_ERR	KERN_SOH "3"	/* error conditions */
#define KERN_WARNING	KERN_SOH "4"	/* warning conditions */
#define KERN_NOTICE	KERN_SOH "5"	/* normal but significant condition */
#define KERN_INFO	KERN_SOH "6"	/* informational */
#define KERN_DEBUG	KERN_SOH "7"	/* debug-level messages */

#define KERN_DEFAULT	""		/* the default kernel loglevel */

/*所以 printk() 可以这样用：printk(KERN_INFO "Hello, world!\n");。*/

/*如果要想在内核启动过程中打印少的信息，就可以根据自己的需要在 kernel/printk.c 中修改以上数值，重新编译即可！*/

/*了解了上面的这些知识后，*/
/*我们就应该知道如何手动控制 printk 打印了。*/
/*例如，我想屏蔽掉所有的内核 printk 打印，那么我只需要把第一个数值调到最小值1或者0。*/

/*# echo 1 4 1 7 > /proc/sys/kernel/printk*/

/*或者*/

/*# echo 0 4 0 7 > /proc/sys/kernel/printk */
```
