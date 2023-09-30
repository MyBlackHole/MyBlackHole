# gpio

[细节](https://www.cnblogs.com/TaXueWuYun/p/15452253.html)

- 常用以下公式计算引脚
```shell
GPIOn_Dx 
group = n - 1
bank = x - 1

GPIO 小组编号计算公式：number = group * 8 + X
GPIO pin脚计算公式：pin = bank * 32 + number


下面演示GPIO4_D5 pin脚计算方法：
bank = 4;      //GPIO4_D5 => 4, bank ∈ [0,4]
group = 3;      //GPIO4_D5 => 3, group ∈ {(A=0), (B=1), (C=2), (D=3)}
X = 5;       //GPIO4_D5 => 5, X ∈ [0,7]
number = group * 8 + X = 3 * 8 + 5 = 29
pin = bank*32 + number= 4 * 32 + 29 = 157;


cd /sys/class/gpio
for i in gpiochip* ; do echo `cat $i/label`: `cat $i/base` ; done
<!-- 0 -> A -->
<!-- 1 -> B -->
<!-- 2 -> C -->
<!-- ... -->

nxp-gpio.0: 0         -> GPIOA
nxp-gpio.4: 128         -> GPIOE  ->AliveGPIO3
nxp-gpio.5: 160         -> GPIOF  ->AliveGPIO5
nxp-gpio.1: 32         -> GPIOB
nxp-gpio.2: 64         -> GPIOC
nxp-gpio.3: 96         -> GPIOD
```

```shell
<!-- 查看中断是否存在 -->
cat /proc/interrupts

<!-- 查看中断执行次数 -->
<!-- xx 指中断号 -->
cat /proc/irq/xx/spurious

<!-- 查看 chip -->
ls /sys/class/gpio
export       gpio440      gpiochip352  gpiochip384  gpiochip416  gpiochip448  gpiochip480  unexport

ls /sys/bus/gpio/devices/
gpiochip0  gpiochip1  gpiochip2  gpiochip3  gpiochip4


:/ # ls /sys/class/gpio/
export     gpiochip128  gpiochip32   gpiochip64  unexport
gpiochip0  gpiochip255  gpiochip500  gpiochip96
:/ # echo 157 > /sys/class/gpio/export
:/ # ls /sys/class/gpio/
export   gpiochip0    gpiochip255  gpiochip500  gpiochip96
gpio157  gpiochip128  gpiochip32   gpiochip64   unexport
:/ # ls /sys/class/gpio/gpio157
active_low  device  direction  edge  power  subsystem  uevent  value
:/ # cat /sys/class/gpio/gpio157/direction
in
:/ # cat /sys/class/gpio/gpio157/value
```


|标志|功能|
|---|---|
|IRQF_SHARED|多个设备共享一个中断线，申请中断函数的dev参数是区分它们的唯一标志|
|IRQF_ONESHOT|单次中断，中断执行一次就结束|
|IRQF_TRIGGER_NONE|无触发|
|IRQF_TRIGGER_RISING|上升沿触发|
|IRQF_TRIGGER_FALLING|下降沿触发|
|IRQF_TRIGGER_HIGH|高电平触发|
|IRQF_TRIGGER_LOW|低电平触发|


## 核心架构
GPIO核心框架是GPIO子系统的核心部分，它提供了与GPIO相关的核心功能
它定义了GPIO的状态管理、中断处理、设备模型等基本机制
GPIO核心框架中的关键数据结构包括gpio_chip、gpio_desc和gpio_irq_chip。

### gpio_chip
gpio_chip是GPIO子系统的关键数据结构之一，表示一个GPIO控制器
每个gpio_chip实例对应一个物理或虚拟的GPIO控制器，负责管理一组GPIO引脚
gpio_chip结构体中包含了一组函数指针，用于执行GPIO相关的操作，例如读取输入状态、设置输出状态等
每个gpio_chip都关联着一组gpio_desc结构体，用于描述和管理具体的GPIO引脚。

### gpio_desc
gpio_desc结构体用于描述和管理GPIO引脚
它包含了GPIO的标识符、状态信息、硬件相关的寄存器地址等
通过gpio_desc，GPIO子系统可以对GPIO引脚进行具体的操作，如读取输入状态、设置输出状态
