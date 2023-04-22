# 系统配置

- 修改最大文件打开数
```shell
# centos
nvim /etc/security/limits.conf
添加
* soft nofile 65535
sysctl -p # 使配置生效

# ubuntu
nvim /etc/security/limits.conf
添加
* soft nofile 65535
reboot # 需要注销才生效
```

- 关闭安全子系统
```shell
# 临时关闭
setenforce 0

# 永久生效
cat > /etc/selinux/config << EOF
SELINUX=disabled
SELINUXTYPE=targeted
EOF
```

- 时间同步
```shell
# 设置同步时区
timedatectl set-timezone Asia/Shanghai
# 开启时间自动同步
timedatectl set-ntp yes ?
```

- ubuntu 开机自动登陆用户
```shell
sudo nvim /etc/gdm3/custom.conf
# 修改
AutomaticLoginEnable = true
AutomaticLogin = <user name>
```

- 给文件或文件夹加锁、解锁
```shell
# 加锁
chattr +i 1.txt

# 解锁
chattr -i 1.txt
```

- 安装自签证 SSL 证书
```shell
# 上传根证书(Root CA)到 /usr/local/share/ca-certificates/ (需要使用.crt扩展名)
sudo nvim /usr/local/share/ca-certificates/feitsui.crt
# 更新 CA certificates
sudo update-ca-certificates
# Root CA应该出现在 /etc/ssl/certs/ca-certificates.crt 底部。
tail /etc/ssl/certs/ca-certificates.crt -n 50

```

- 设置触摸盘
```shell
<!-- /etc/X11/xorg.conf.d $ cat 70-synaptics.conf  -->
<!-- Section "InputClass" -->
<!--         Identifier "touchpad" -->
<!--         Driver "synaptics" -->
<!--         MatchIsTouchpad "on" -->
<!--                 Option "TapButton1" "1" -->
<!--                 Option "TapButton2" "3" -->
<!--                 Option "TapButton3" "0" -->
<!--                 Option "VertEdgeScroll" "on" -->
<!--                 Option "VertTwoFingerScroll" "on" -->
<!--                 Option "HorizEdgeScroll" "on" -->
<!--                 Option "HorizTwoFingerScroll" "on" -->
<!--                 Option "VertScrollDelta" "-112" -->
<!--                 Option "HorizScrollDelta" "-114" -->
<!--                 Option "MaxTapTime" "125" -->
<!-- EndSection -->


sudo apt install xserver-xorg-input-libinput
Section "InputClass"
        Identifier "libinput touchpad catchall"
        Driver "libinput"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Option "Tapping" "on"
        Option "NaturralScrolling" "true"
        Option "ClickMethod" "clickfinger"
EndSection
```

- 修改亮度
```shell
# 不同显卡 amdgpu_bl0 不同
echo 255 > /sys/class/backlight/amdgpu_bl0/brightness

# 获取最大亮度
cat /sys/class/backlight/amdgpu_bl0/max_brightness
```
