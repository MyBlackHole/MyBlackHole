# grub

- install
```shell
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=archlinux

grub-mkconfig -o /boot/grub/grub.cfg
```
