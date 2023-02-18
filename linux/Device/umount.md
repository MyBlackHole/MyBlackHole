### 描述
**卸载文件系统(确保没有程序在使用此设备资源)**

### 语法
|  参数 | 说明   |
|-------------- | -------------- |
|-a, --all |卸载所有文件系统 |
|-A, --all-targets |卸载当前名字空间内指定设备对应的所有挂臷点 |
|-c, --no-canonicalize |不对路径规范化 |
|-d, --detach-loop |若挂臷了回环设备，也释放该回环设备 |
|--fake |空运行；跳过 umount(2) 系统调用 |
|-f, --force |强制卸载(遇到不响应的 NFS 系统时) |
|-i, --internal-only |不调用 umount.<类型> 辅助程序 |
|-n, --no-mtab |不写 /etc/mtab |
|-l, --lazy |立即断开文件系统，清理以后执行 |
|-O, --test-opts <列表> |限制文件系统集合(和 -a 选项一起使用) |
|-R, --recursive |递归卸载目录及其子对象 |
|-r, --read-only |若卸载失败，尝试以只读方式重新挂臷 |
|-t, --types <列表> |限制文件系统集合 |
|-v, --verbose |打印当前进行的操作 |
|-q, --quiet |suppress 'not mounted' error messages |
|-N, --namespace |perform umount in another namespace |
|-h, --help |display this help |
|-V, --version |display version |

### 例子
|  命令 | 说明   |
|-------------- | -------------- |
|Sudo umount -t cift //192.168.2.246/e |卸载共享文件夹 |
