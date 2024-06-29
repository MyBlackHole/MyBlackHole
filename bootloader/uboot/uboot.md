# uboot

## 常用命令
```shell
version             #查看uboot版本
reset               #重启Uboot

printenv            #打印uboot环境变量
setenv name value   #设置环境变量


md addr             #查看内存指令
nm addr             #修改内存值
mm addr             #自增修改内存值

mmc dev id          #选择mmc卡
mmc rescan          #扫描卡

echo $name          #打印环境变量
```

- 进入 uboot 命令模式
```shell
<!-- 打开我们准备好一份Uboot源码，进入menuconfig配置菜单，主要设置下列几个配置信息！ -->
CONFIG_CMDLINE：命令行模式开关
CONFIG_SYS_PROMPT：命令行模式提示符
CONFIG_HUSH_PARSER：使用hush shell 来对命令进行解析
BOOTDELAY：设置启动延时
Tip：meneconfig中查找苦难？实时/符号，输入1或2或3，直接查找指定标识。
```
