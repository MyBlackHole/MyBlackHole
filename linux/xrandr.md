# xrandr
扩展屏幕设置

## 例子

- 屏幕列表
```shell
xrandr --listmonitors
Monitors: 2
 0: +*eDP 1920/309x1080/174+0+0  eDP
 1: +HDMI-A-0 1920/527x1080/296+1920+0  HDMI-A-0
```

- 屏幕旋转
```shell
# 'normal', 'left', 'right' or 'inverted'
xrandr --output HDMI-A-0 --rotate left
```

- 设置屏幕摆放位置（调整成左右屏、上下屏）
```shell
# --left-of, --right-of, --above, --below, --same-as
xrandr --output eDP --right-of HDMI-A-0
```

- 设置主屏幕
```shell
xrandr --output eDP --primary
```

- 打开外接显示器，双屏幕显示相同的内容--克隆，（auto为最高分辨率）
```shell
xrandr --output HDMI-A-0 --same-as eDP --auto
```

- 若要指定外接显示器的分辨率可以使用下面的命令(1280*1024)
```shell
xrandr --output HDMI-A-0 --same-as eDP --mode 1280x1024
```

- 打开外接显示器，设置为右侧扩展
```shell
xrandr --output HDMI-A-0 --right-of eDP --auto
```

- 关闭显示器
```shell
xrandr --output HDMI-A-0 --off
```

- 打开VGA-0接口显示器，关闭LVDS接口显示器
```shell
xrandr --output HDMI-A-0 --auto --output eDP --off
```

- 自动 (镜像)
```shell
xrandr --output eDP --auto --output HDMI-A-0 --auto
```
