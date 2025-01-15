# linux


## 目录
/dev /proc /sys 三个文件都是在内存上的 
/proc     存放系统信息， cpu，mem， devices，interupt
          cat /proc/devices
          cat /proc/interrupt
          cat /proc/1/schedstat

/dev      存放设备节点的(ls -l /dev/xxx 查看主次设备号)

/sys      存放驱动的信息（平台总线）： class_create , device_create
          /sys/dev                block,char 主次设备号

          /sys/bus/xxx（各种总线）      device，driver注册到不同的总线：platform、i2c、usb...
                     （uevent：driver、device匹对的名字）

          /sys/devices（源头）      记录系统所有的设备 （struct devices ）

          /sys/class/<类名>/xxx    分类查看驱动信息：
          cd  /sys/class/input/event0/device
          /sys/class/input/event0/device$  cat name
           Power Button    


说明：device、driver都可以在/sys/bus  的总线上找到device、dirver的信息。
     /sys/device 是所以devices的源头，driver可以在 /sys/bus/xxx/driver中找到。

    创建设备节点的时候:
        device_create();
        在/sys/class/xxx/uevent   //uevent记录主次设备号，name
    mdev -s  (手动挂载 mknod /dev/xxx c  主  次)
        /sys/class/xxx/uevent    //遍历/sys/class/下的所有uevent,把名字挂载为/dev/xxx 


## 例子

- 查看已使用设备号
```shell
cat /proc/devices
```
