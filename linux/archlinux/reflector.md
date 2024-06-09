# reflector

arch 源管理

## install
```shell
sudo pacman -S reflector
```


## demo
```shell
sudo reflector --sort rate --threads 100 -c China --save /etc/pacman.d/mirrorlist
```
