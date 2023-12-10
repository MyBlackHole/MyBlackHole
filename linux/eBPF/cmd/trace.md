# trace

跟踪特定过程函数

```shell
sudo /usr/share/bcc/tools/trace -p 4095 'do_sys_open "%s", arg2'

TIME     PID    COMM         FUNC             -
12:17:14 15437  firefox      do_sys_open      /run/user/1000/dconf/user
12:17:14 15437  firefox      do_sys_open      /home/tecmint/.config/dconf/user
12:18:07 15437  firefox      do_sys_open      /run/user/1000/dconf/user
12:18:07 15437  firefox      do_sys_open      /home/tecmint/.config/dconf/user
12:18:13 15437  firefox      do_sys_open      /sys/devices/system/cpu/present
12:18:13 15437  firefox      do_sys_open      /dev/urandom
12:18:13 15437  firefox      do_sys_open      /dev/urandom
12:18:14 15437  firefox      do_sys_open      /usr/share/fonts/truetype/liberation/LiberationSans-Italic.ttf
12:18:14 15437  firefox      do_sys_open      /usr/share/fonts/truetype/liberation/LiberationSans-Italic.ttf
12:18:14 15437  firefox      do_sys_open      /usr/share/fonts/truetype/liberation/LiberationSans-Italic.ttf
12:18:14 15437  firefox      do_sys_open      /sys/devices/system/cpu/present
12:18:14 15437  firefox      do_sys_open      /dev/urandom
12:18:14 15437  firefox      do_sys_open      /dev/urandom
12:18:14 15437  firefox      do_sys_open      /dev/urandom
12:18:14 15437  firefox      do_sys_open      /dev/urandom
12:18:15 15437  firefox      do_sys_open      /sys/devices/system/cpu/present
12:18:15 15437  firefox      do_sys_open      /dev/urandom
12:18:15 15437  firefox      do_sys_open      /dev/urandom
12:18:15 15437  firefox      do_sys_open      /sys/devices/system/cpu/present
12:18:15 15437  firefox      do_sys_open      /dev/urandom
12:18:15 15437  firefox      do_sys_open      /dev/urandom
....
```
