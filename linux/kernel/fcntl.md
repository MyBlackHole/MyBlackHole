net/fcntl.c:fcntl
    net/fcntl.c:do_fcntl
        net/fcntl.c:setfl

### fcntl系统调用

功能描述：根据文件描述词来操作文件的特性。

用法：

1. int fcntl(int fd, int cmd);   
     
2. int fcntl(int fd, int cmd, long arg);   
     
3. int fcntl(int fd, int cmd, struct flock *lock); 

参数：

- fd：文件描述词。
- cmd：操作命令。
- arg：供命令使用的参数。
- lock：同上。

有以下操作命令可供使用

一、F_DUPFD：复制文件描述词。

二、FD_CLOEXEC ：设置close-on-exec标志。

如果FD_CLOEXEC位是0，执行execve的过程中，文件保持打开。反之则关闭。

三、F_GETFD：读取文件描述词标志。 

四、F_SETFD：设置文件描述词标志。 

五、F_GETFL：读取文件状态标志。 

六、F_SETFL：设置文件状态标志。

其中O_RDONLY， O_WRONLY， O_RDWR， O_CREAT，  O_EXCL， O_NOCTTY 和 O_TRUNC不受影响，能更改的标志有 O_APPEND，O_ASYNC， O_DIRECT， O_NOATIME 和 O_NONBLOCK。

七、F_GETLK, F_SETLK 和 F_SETLKW ：获取，释放或测试记录锁，使用到的参数是以下结构体指针：

- F_SETLK：在指定的字节范围获取锁（F_RDLCK, F_WRLCK）或释放锁（F_UNLCK）。如果和另一个进程的锁操作发生冲突，返回 -1并将errno设置为EACCES或EAGAIN。
- F_SETLKW：行为如同F_SETLK，除了不能获取锁时会睡眠等待外。如果在等待的过程中接收到信号，会即时返回并将errno置为EINTR。
- F_GETLK：获取文件锁信息。
- F_UNLCK：释放文件锁。

为了设置读锁，文件必须以读的方式打开。为了设置写锁，文件必须以写的方式打开。为了设置读写锁，文件必须以读写的方式打开。

八、信号管理 

F_GETOWN, F_SETOWN, F_GETSIG 和 F_SETSIG 被用于IO可获取的信号。

- F_GETOWN：获取当前在文件描述词 fd上接收到SIGIO 或 SIGURG事件信号的进程或进程组标识 。
- F_SETOWN：设置将要在文件描述词fd上接收SIGIO 或 SIGURG事件信号的进程或进程组标识 。
- F_GETSIG：获取标识输入输出可进行的信号。
- F_SETSIG：设置标识输入输出可进行的信号。

使用以上命令，大部分时间程式无须使用select()或poll()即可实现完整的异步I/O。

九、租约（ Leases） 

1、F_SETLEASE 和 F_GETLEASE 被用于当前进程在文件上的租约。文件租约提供当一个进程试图打开或折断文件内容时，拥有文件租约的进程将会被通告的机制。

2、F_SETLEASE：根据以下符号值设置或删除文件租约

- F_RDLCK设置读租约，当文件由另一个进程以写的方式打开或折断内容时，拥有租约的当前进程会被通告。
- F_WRLCK设置写租约，当文件由另一个进程以读或以写的方式打开或折断内容时，拥有租约的当前进程会被通告。
- F_UNLCK删除文件租约。

3、F_GETLEASE：获取租约类型。

十、文件或目录改动通告 

（linux 2.4以上）当fd索引的目录或目录中所包含的某一文件发生变化时，将会向进程发出通告。arg参数指定的通告事件有以下，两个或多个值能通过或运算组合。

- DN_ACCESS 文件被访问 (read, pread, readv)
- DN_MODIFY 文件被修改(write, pwrite,writev, truncate, ftruncate)
- DN_CREATE 文件被建立(open, creat, mknod, mkdir, link, symlink, rename)
- DN_DELETE 文件被删除(unlink, rmdir)
- DN_RENAME 文件被重命名(rename)
- DN_ATTRIB 文件属性被改动(chown, chmod, utime[s])

返回说明：

成功执行时，对于不同的操作，有不同的返回值

- F_DUPFD：新文件描述词
- F_GETFD：标志值
- F_GETFL：标志值
- F_GETOWN：文件描述词属主
- F_GETSIG：读写变得可行时将要发送的通告信号，或0对于传统的SIGIO行为

对于其他命令返回0。

失败返回-1，errno被设为以下的某个值

- EACCES/EAGAIN: 操作不被允许，尚未可行
- EBADF: 文件描述词无效
- EDEADLK: 探测到可能会发生死锁
- EFAULT: 锁操作发生在可访问的地å址空间外
- EINTR: 操作被信号中断
- EINVAL： 参数无效
- EMFILE: 进程已超出文件的最大可使用范围
- ENOLCK: 锁已被用尽
- EPERM:权能不允许

1. struct flock {  
     
2.         short l_type; /* 锁类型： F_RDLCK, F_WRLCK, F_UNLCK */  
     
3.         short l_whence; /* l_start字段参照点： SEEK_SET(文件头), SEEK_CUR(文件当前位置), SEEK_END(文件尾) */  
     
4.         off_t l_start; /* 相对于l_whence字段的偏移量 */  
     
5.           off_t l_len; /* 需要锁定的长度 */  
     
6.         pid_t l_pid; /* 当前获得文件锁的进程标识（F_GETLK） */  
     
7. };

