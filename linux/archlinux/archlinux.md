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

pacstrap /mnt base base-devel linux linux-firmware grub efibootmgr iwd neovim git

genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Arch
grub-mkconfig -o /boot/grub/grub.cfg
```

## config

- amd gpu driver
```shell
paru -S mesa lib32-mesa xf86-video-amdgpu vulkan-radeon lib32-vulkan-radeon
```

