- reason given by server: No sudo file or directory
```shell
nfs4 版本 
Please do not specify the server path /data for NFSv4. You need to specify only / as fsid is set to 0。就是nfs4不需要指定服务器路径/数据，只需要指定虚拟根目录“/”就可以，因为FSID已经设置为0。

<!-- 有可能版本不支持 -->
cat /boot/config-$(uname -r)|grep "CONFIG_NFS_V"
```

- clnt_create: RPC: Unknown host
```shell
systemctl start rpcbind.service
systemctl start nfs-server.service
systemctl start rpcsvcgssd.service
```

- Static declaration of 'find_key_by_type_and_desc' follows non-static declaration
```shell
# 声明的文件
#include <keyutils.h>

# 实现, (多个 static, 去掉)
static key_serial_t find_key_by_type_and_desc(const char *type,
```
