### 异常处理

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

<<<<<<< HEAD
- fatal error: linux/smp_lock.h: No such file or directory
```shell
# 替换成
#include <linux/hardirq.h>
=======
- undefined reference to `major‘
```c
#include <sys/types.h>
#include <sys/sysmacros.h>
```

- Fatal error: rpc/rpc.h: No such file or directory
```shell
-I/usr/include/tirpc
>>>>>>> 667cddfe50b488f7b3ec7e69e8c6e602917a6114
```
