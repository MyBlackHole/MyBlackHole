# task_struct

## uid\euid

uid(真实id): 进程创建者 id
euid: 进程对文件访问权限

### SUID 

1. 只对二进制程序有效,对shell脚本或者目录无效
2. 拥有者的权限执行位改为s
3. 执行者拥有x权限
4. 执行者在`运行程序过程中`将获得改文件`拥有者的权限`


### SGID

1. 用于二进制程序与SUID类似，不过获得的是群组的权限
2. 用与目录时，若用户对于该目录具有rx权限，则用户可以进入该目录，用户在该目录下的有效群组将变成目录的群组，
若用户对目录具有w权限，则用户在该目录下创建的文件群组为该目录的群组，这对于协同工作十分有益


### SBIT

1. 只对目录有效
2. 若用户对于目录具有wx权限，则只有root和用户本人才能删除用户在该目录下创建的文件

#### 修改特殊权限

用额外一位数字表示

例如:

`chmod 4755 file` = `chmod u+s file`

`chmod 2765 file` = `chmod g+s file`

`chmod 1777 file` = `chmod o+t, file`

注意，若对于文件没有x权限，则对应位置会变成大写的S或者T `chmod 1754 dir` => `drwxr-xr-T`

- suid 案例
```shell
设置 suid 程序路径在移动设备挂载点上失效
例如：
/dev/sda1 on /media/black/Data type ext4 (rw,nosuid,nodev,relatime,errors=remount-ro,uhelper=udisks2)

<!-- printid.c -->
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main(void)
{
    printf(" UID\t= %d\n", getuid());
    printf(" EUID\t= %d\n", geteuid());
    printf(" GID\t= %d\n", getgid());
    printf(" EGID\t= %d\n", getegid());

    return EXIT_SUCCESS;
}

gcc -o printid printid.c

./printid
UID        = 1000
UID        = 1000
GID        = 100
GID        = 100


sudo ./printid
 UID    = 0
 EUID   = 0
 GID    = 0
 EGID   = 0


<!-- 文件在执行阶段具有文件所有者的权限 -->
chmod u+s printid

<!-- 文件在执行阶段具有文件所属组的权限 -->
chmod g+s printid


./printid
 UID    = 1000
 EUID   = 1000
 GID    = 1000
 EGID   = 1000

使用 root 执行
sudo ./printid
 UID    = 0
 EUID   = 0
 GID    = 0
 EGID   = 0

<!-- test 用户 -->
./getuid_test1
 UID    = 1001
 EUID   = 1001
 GID    = 1001
 EGID   = 1001
```
