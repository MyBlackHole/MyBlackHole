# config

```shell
git clone git@github.com:stav121/i3wm-themer.git
cd i3wm-themer
python3 i3wm-themer.py --config defaults/config.yaml --install defaults/
python3 i3wm-themer.py --config defaults/config.yaml --load themes/007.json

git clone git@github.com:adi1090x/polybar-themes.git
cd polybar-themes
./setup.sh
[*] Installing Polybar Themes...

[*] Choose Style -
[1] Simple
[2] Bitmap

[?] Select Option : 2

[*] Installing fonts...
[*] Creating a backup of your polybar configs...
[*] Successfully Installed.

sudo apt install mpd battery-stats
```


## 系统配置

- 文件系统挂载信息（实时）
```shell
/proc/self/mounts
```

- 获取系统发行版信息
```shell
cat /etc/os-release
```

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

- 自动登陆
```shell
sudo vim /etc/gdm3/custom.conf 

[daemon] 

AutomaticLoginEnable=True(true 开启，false关闭) 

AutomaticLogin=sunriseydy
```

- 设置默认文本编辑器
```shell
sudo update-alternatives --config editor
```

- git
```shell
[http] 

    proxy=http://127.0.0.1:1081/ 

[https] 

    proxy=http://127.0.0.1:1081/ 

[user] 

    name = [1358244533@qq.com](mailto:1358244533@qq.com) 

    email = [1358244533@qq.com](mailto:1358244533@qq.com) 

    password = ***************** 

[core] 

    autocrlf = input 

    quotepath = false 

[credential] 

    helper = store
```

- 代理设置
```shell
Apt 

临时 

Sudo apt -o Acquire::ftp::proxy="ftp://127.0.0.1:8087/" install xxxx 

永久 

apt 代理 

临时 

 sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:1080" install festival festvox-kallpc16k 

永久 

/etc/apt/apt.conf 

Acquire::ftp::proxy "[ftp://127.0.0.1:8087/](ftp://127.0.0.1:8087/)";  

Acquire::http::proxy "[http://127.0.0.1:8087/](http://127.0.0.1:8087/)";  

Acquire::https::proxy "[https://127.0.0.1:8087/](https://127.0.0.1:8087/)";  

Acquire::socks::proxy "[https://127.0.0.1:8087/](https://127.0.0.1:8087/)"; 

/etc/bash.bashrc 

export ftp_proxy="ftp://user:password@proxyIP:port" 

export http_proxy="http://user:password@proxyIP:port"  

export https_proxy="https://user:password@proxyIP:port" 

export socks_proxy="https://user:password@proxyIP:port"
```

- 网络配置
```shell
# 临时
dhclient eth0 

ifconfig eth0 192.168.1.11/24 #设置网络接口的ip地址(子网掩码) 

route add default gw 192.168.1.1 #添加默认网关路由 

echo nameserver 192.168.1.1 > /etc/resolv.conf #dns服务器
# 永久
cat /etc/network/interface 

auto eth0 

iface eth0 inet static #静态 

address 192.168.20.1 #ip设置 

netmask 255.255.255.0 #子网掩码 

network 192.168.20.0 

broadcast 192.168.20.255 

gateway 192.168.20.2 #网关  

dns-nameservers 192.168.1.1 192.168.1.2 #dns服务器 

up route add -net 172.16.5.0/24 gw 192.168.10.100 eth1 down route del -net 172.24.0.0/24
```

- 笔记本模式
```shell
#!/bin/bash 

currentMode=$(cat /proc/sys/vm/laptop_mode) 

if [ $currentMode -eq 0 ] 

then 

echo "5" > /proc/sys/vm/laptop_mode echo "Laptop Mode Enabled" 

else 

echo "0" > /proc/sys/vm/laptop_mode echo "Laptop Mode Disabled" 

fi
```

- 开启硬盘省电选项
```shell
hdparm -i /dev/sda if AdvancedPM=yes then hdparm -B 1 -S 12 /dev/ sda
```

- centos 添加源
```shell
yum install epel-release
```

- 替换清华源
```shell
sudo cat > /etc/apt/sources.list << EOF 

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释 

deb [http://mirrors.tuna.tsinghua.edu.cn/ubuntu/](https://mirrors.tuna.tsinghua.edu.cn/ubuntu/) bionic main restricted universe multiverse 

deb [http://mirrors.tuna.tsinghua.edu.cn/ubuntu/](https://mirrors.tuna.tsinghua.edu.cn/ubuntu/) bionic-updates main restricted universe multiverse 

deb [http://mirrors.tuna.tsinghua.edu.cn/ubuntu/](https://mirrors.tuna.tsinghua.edu.cn/ubuntu/) bionic-backports main restricted universe multiverse 

deb [http://mirrors.tuna.tsinghua.edu.cn/ubuntu/](https://mirrors.tuna.tsinghua.edu.cn/ubuntu/) bionic-security main restricted universe multiverse 

EOF 

(无反应把https的s曲调)
```

- 声卡
```shell
sudo apt-get install alsa-base alsa-utils alsa-oss alsa-tools
或
sudo apt install pulseaudio
```

- 查询大小端
```shell
<!-- 大端序（大端模式）：是指数据的低位保存在内存的高地址中，而数据的高位保存在内存的低地址中。 -->
<!-- 小端序（小端模式）：是指数据的低位保存在内存的低地址中，而数据的高位保存在内存的高地址中。 -->

echo -n I | od -o | head -n1 | cut -f2 -d" " | cut -c6
输出：1为小端模式，0为大端模式；
解析：od命令的作用为将指定内容以八进制、十进制、十六进制、浮点格式或ASCII编码字符方式显示；
```

