# arch

## install
```shell
iwctl
station wlan0 connect <ssid>

cfdisk /dev/sda
efi 2G
root xxx

mount /dev/sda1 /mnt
mount /dev/sda2 /mnt/boot

pacstrap /mnt base base-devel linux linux-firmware grub efibootmgr iwd neovim git dhcpd

genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt

<!-- 修改 root 密码: 需要操作不然无法进入系统 -->
passwd

useradd -m black
passwd black

<!-- 默认启用 iwd 服务 -->
systectl enable iwd

<!-- 给 black 用户添加 sudo 权限 -->
nvim /etc/sudoers
black ALL=(ALL) ALL


grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Arch
grub-mkconfig -o /boot/grub/grub.cfg

<!-- 切换到 black 用户 -->
su - black

cd /run/media/black/Data/Documents/github/shell/Hyprdots/Scripts/
./install.sh
<!-- 启动服务 -->
./install.sh s

ssh-keygen -t rsa -C '1358244533@qq.com' 

git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp true

reboot
```

## config

- amd gpu driver
```shell
paru -S mesa lib32-mesa xf86-video-amdgpu vulkan-radeon lib32-vulkan-radeon
```

