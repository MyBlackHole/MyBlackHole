# struct

1. 进程相关数据结构
    1) struct task_struct
    2) struct cred 
    3) struct pid_link
    4) struct pid 
    5) struct signal_struct 
    6) struct rlimit
2. 内核中的队列/链表对象
    1) singly-linked lists
    2) singly-linked tail queues
    3) doubly-linked lists
    4) doubly-linked tail queues
3. 内核模块相关数据结构
    1) struct module 
4. 文件系统相关数据结构
    1) struct file
    2) struct inode 
    3) struct stat
    4) struct fs_struct 
    5) struct files_struct
    6) struct fdtable 
    7) struct dentry 
    8) struct vfsmount
    9) struct nameidata
    10) struct super_block
    11) struct file_system_type
5. 内核安全相关数据结构
    1) struct security_operations
    2) struct kprobe
    3) struct jprobe
    4) struct kretprobe
    5) struct kretprobe_instance 
    6) struct kretprobe_blackpoint 、struct kprobe_blacklist_entry 
    7) struct linux_binprm
    8) struct linux_binfmt 
6. 系统网络状态相关的数据结构
    1) struct ifconf
    2) struct ifreq 
    3) struct socket
    4) struct sock
    5) struct proto_ops
    6) struct inet_sock
    7) struct sockaddr     
7. 系统内存相关的数据结构
    1) struct mm_struct
    2) struct vm_area_struct
    3) struct pg_data_t
    4) struct zone
    5) struct page
8. 中断相关的数据结构
    1) struct irq_desc
    2) struct irq_chip
    3) struct irqaction
9. 进程间通信(IPC)相关数据结构
    1) struct ipc_namespace
    2) struct ipc_ids
    3) struct kern_ipc_perm
    4) struct sysv_sem
    5) struct sem_queue
    6) struct msg_queue 
    7) struct msg_msg
    8) struct msg_sender
    9) struct msg_receiver
    10) struct msqid_ds
10. 命名空间(namespace)相关数据结构
    1) struct pid_namespace 
    2) struct pid、struct upid
    3) struct nsproxy
    4) struct mnt_namespace


## struct task_struct
![[imgs/struct.png]]

![[imgs/struct-1.png]]

R: 程序正在运行或者是在等待运行的队列中
S: 处于休眠状态的进程，等待触发事件。
D: 不接受任何异步信号的休眠状态。通常是在处理一些不可中断的任务。
Z: 程序已经终止。等待父进程回收。这种进程不再占用内存和CPU。
