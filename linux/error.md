### 异常处理

- libcharset.so.1 is not a symbolic link
```shell
因为 libcharset.so.1 正常情况下应该是一个符号链接,而不是实体文集件,修改其为符号链接即可
ln -sf libcharset.so libcharset.so.1
```

- 终端回车变 ^m使用 reset 命令 

- netease-cloud-music 
```sh
/opt/netease/netease-cloud-music/netease-cloud-music: /opt/netease/netease-cloud-music/libs/libselinux.so.1: no version information available (required by /lib/x86_64-linux-gnu/libgio-2.0.so.0)
/opt/netease/netease-cloud-music/netease-cloud-music: symbol lookup error: /lib/x86_64-linux-gnu/libgio-2.0.so.0: undefined symbol: g_module_open_full

# 解决
sudo nvim /opt/netease/netease-cloud-music/netease-cloud-music.bash
## 添加以下在 exec 前
cd /lib/x86_64-linux-gnu/
```

- fatal error: linux/smp_lock.h: No such file or directory
```shell
# 替换成
#include <linux/hardirq.h>
```

- undefined reference to `major‘
```c
#include <sys/types.h>
#include <sys/sysmacros.h>
```

- Fatal error: rpc/rpc.h: No such file or directory
```shell
-I/usr/include/tirpc
```

- mount.nfs: failed to apply fstab options
```shell
sudo mount -t nfs 192.168.2.77:/tmp/db1_bin /tmp/db2_bin
```

- fatal error: sys/capability.h: No such file or directory
```shell
sudo apt install libcap-dev
```

- error: ImageMagick not installed? Executable 'convert' not found
```shell
sudo apt install imagemagick
```

- error: gifsicle not installed? Executable 'gifsicle' not found
```shell
sudo apt install gifsicle
# npm install image-webpack-loader -D
```

- Could NOT find Gettext
```shell
sudo apt install gettext
```

- gssapi/gssapi.h: No such file or directory
```shell
sudo apt install libkrb5-dev
```

- DECLARE_ASN1_DUP_FUNCTION does not have a number assigned 
```shell
安装 openssl 1.1.1
openssl 3.0 版本遗漏
git clean -df 
make clean
./config --prefix=/media/black/Data/lib --openssldir=/media/black/Data/lib/openssldir
perl configdata.pm --dump
make -j8
```
