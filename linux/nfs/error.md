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
