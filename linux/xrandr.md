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
