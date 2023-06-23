# kernel

## source 安装
```shell
sudo apt-get install kernel-source-xxxx
```

## 生成 compile_commands.json
```shell
scripts/clang-tools/gen_compile_commands.py
```

- 清理全部 make 的产物
```shell
make mrproper
```

- 更换内核
```shell
git clone [https://github.com/pimlie/ubuntu-mainline-kernel.sh.git](https://github.com/pimlie/ubuntu-mainline-kernel.sh.git)

cd ubuntu-mainline-kernel.sh/

sudo mv ubuntu-mainline-kernel.sh /usr/local/bin/

sudo ubuntu-mainline-kernel.sh -i v5.11.0#下载5.11.0版本内核，可指定其他版本

sudo ubuntu-mainline-kernel.sh -u #删除不需要的版本，这样就可以留下需要版本，实现版本随意升级甚至降级

重启

1.安装需要的内核版本：

apt install linux-image-5.4.0-1029-aws

2.列出可用的ubuntu内核版本：

grep 'menuentry \|submenu ' /boot/grub/grub.cfg | cut -f2 -d "'"

3.备份一下grub文件：

cp /etc/default/grub /etc/default/grub.bak

4.修改内核版本：

vim /etc/default/grub

GRUB_DEFAULT='Advanced options for Ubuntu>Ubuntu, with Linux 5.4.0-1029-aws'

#根据上面可用的内核版本进行设置

update-grub 

#更新

5.重启！之后uname -r 查看当前内核版本。
```