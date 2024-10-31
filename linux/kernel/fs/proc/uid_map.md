# uid_map

uid_map 的格式为：
每行包含三个用空格分隔的 32 位无符号整数，分别为（to-user-id-start from-user-id-start range）：
to-user-id-start 如果当前文件为 /proc/[pid]/uid_map，则该值为 [pid] 所在 User Namespace 的用户 ID
from-user-id-start 取决于读取 /proc/[pid]/uid_map 进程所在的 User Namespace（不同 User Namespace 进程读 uid_map 看到的第二列的内容是不一样的。）。
如果和 [pid] 所在的 User Namespace 相同，则 from-user-id-start 表示映射到父 User Namespace 的用户 ID。
如果和 [pid] 所在的 User Namespace 不同，则 from-user-id-start 表示映射读写 /proc/[pid]/uid_map 进程所在的 User Namespace 的用户 ID。
range 表示映射的范围，必须大于 0。

uid_map 写入说明
只能写入一次，也就是说一旦确定则不能修改，刚创建时该文件是空的。
写入必须以换行符结尾。包含多行，Linux 4.14 之前最多 5 行，Linux 4.15 起，最多 340 行，多行中的映射范围不允许有重叠，最少写入 1 行。
写入的进程必须拥有该文件 User Namespace 的 CAP_SETUID (CAP_SETGID) 的 capability 且 写入的进程的 User Namespace 必须是当前 User Namespace 或者 父 User Namespace。
写入的映射的用户 ID（组 ID）必须依次在父用户命名空间。
如果想映射父进程的 0 (即 xxx 0 xxx)，除了满足上述要求外：还要求（Since Linux 5.12，解决一个安全漏洞）：
如果是该 User Namespace 的进程写入，要求创建该 User Namespace 时的父进程必须有的 CAP_SETFCAP capability。
如果是该 User Namespace 的父 User Namespace 的进程写入，要求该父进程必须有的 CAP_SETFCAP capability。
以下两个 case 需要特别说明：
当写入进程有父 User Namespace 的 CAP_SETUID (CAP_SETGID) capability 时，则没有其他限制（按照如上规则。此情况，只有父进程写入常见场景才满足）。
否则，存在如下限制（子进程写入场景）：
写入进程和创建该 User Namespace 的父进程有相同 effective user ID （EUID），且写入的内容必须包含一个映射到父进程的 EUID 的行。
写入在 gid_map 之前，必须通过写入 "deny" 到 /proc/[pid]/setgroups 文件，来禁用 setgroups(2) 系统调用。
综上所述，推荐的模式是，父进程创建完 User Namespace 后，在父进程中写入 id map，然后通过进程通讯技术（如 pipe）通知位于新的 User Namespace 中的子进程。


```shell
❯ cat /proc/self/status|grep Cap
CapInh: 0000000800000000
CapPrm: 0000000000000000
CapEff: 0000000000000000
CapBnd: 000001ffffffffff
CapAmb: 0000000000000000

❯ cat /proc/1/status|grep Cap
CapInh: 0000000000000000
CapPrm: 000001ffffffffff
CapEff: 000001ffffffffff
CapBnd: 000001ffffffffff
CapAmb: 0000000000000000
```
