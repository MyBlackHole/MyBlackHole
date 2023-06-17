# nfsd svc
内核层 nfsd svc

- 文件系统注册
fs/nfsd/nfsctl.c:init_nfsd
    fs/nfsd/nfsctl.c:register_filesystem
        fs/nfsd/nfsctl.c:nfsd_init_fs_context (file_system_type.init_fs_context)

- 挂载 nfsd
fs/nfsd/nfsctl.c:nfsd_init_fs_context (fs_context_operations.nfsd_fs_get_tree)
    fs/nfsd/nfsctl.c:nfsd_fs_get_tree 
        fs/super.c:get_tree_keyed (fill_super = nfsd_fill_super)
            fs/super.c:vfs_get_super (fill_super = nfsd_fill_super)
                fs/super.c:fill_super (nfsd_fill_super)
                    fs/nfsd/nfsctl.c:nfsd_fill_super 
                        fs/libfs.c:simple_fill_super (挂载文件系统与初始化 nfsd_files(提供用户层对内核 nfsd 层配置)) 

- 文件操作 (配置 nfsd)
写 portlist "tcp 20490" 
fs/nfsd/nfsctl.c:nfsctl_transaction_write
    fs/nfsd/nfsctl.c:write_op[NFSD_Ports] (write_ports)
        fs/nfsd/nfsctl.c:write_ports
            fs/nfsd/nfsctl.c:__write_ports
                fs/nfsd/nfsctl.c:__write_ports_addxprt
                    fs/nfsd/nfsctl.c:nfsd_create_serv (创建 nfsd_program)
                    net/sunrpc/svc_xprt.c:svc_xprt_create
                        net/sunrpc/svc_xprt.c:_svc_xprt_create
                            net/sunrpc/svc_xprt.c:__svc_xpo_create
                                net/sunrpc/svc_xprt.c:__svc_xpo_create
                                    net/sunrpc/svc_xprt.c:xpo_create
                                        net/sunrpc/svcsock.c:svc_tcp_create
                                            net/sunrpc/svcsock.c:svc_create_socket
                                                net/sunrpc/svcsock.c:kernel_bind
                                                net/sunrpc/svcsock.c:kernel_listen

- 连接 socket send data to 20490 port
```c
# 网络层支持
static const struct svc_xprt_ops svc_tcp_ops = {
	.xpo_create = svc_tcp_create,
	.xpo_recvfrom = svc_tcp_recvfrom,
	.xpo_sendto = svc_tcp_sendto,
	.xpo_result_payload = svc_sock_result_payload,
	.xpo_release_rqst = svc_tcp_release_rqst,
	.xpo_detach = svc_tcp_sock_detach,
	.xpo_free = svc_sock_free,
	.xpo_has_wspace = svc_tcp_has_wspace,
	.xpo_accept = svc_tcp_accept,
	.xpo_kill_temp_xprt = svc_tcp_kill_temp_xprt,
};

此处还有一层 sunrpc 处理层
fs/nfsd/nfsctl.c:nfsd (业务处理： 接收 rqstp, 消费 svc_process(rqstp))
```
