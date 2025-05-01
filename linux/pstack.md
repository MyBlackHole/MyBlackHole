# pstack

查看程序进程栈

## 例子

- 程序进程栈

```shell
[root@dengxizz-worker2 fsclient]# pstack 78238
#0  0x00007fa472257740 in __read_nocancel () from /lib64/libpthread.so.0
#1  0x000000000043e276 in UpdateFileBlock (backupPath="/app/pgBa/192.168.50.89/hA1-I1/_data_pgdata", verStorePath="/app/pgBa/192.168.50.89/hA1_full/_data_pgdata/data", opFile="/data/pgdata/global/pg_control") at restore.cpp:203
#2  0x000000000043d799 in RestoreTargetPath (backupPath="/app/pgBa/192.168.50.89/hA1-I1/_data_pgdata", verStorePath="/app/pgBa/192.168.50.89/hA1_full/_data_pgdata/data", bFullBak=false) at restore.cpp:73
#3  0x000000000043f92a in RestoreBackup (backName="hA1-I1", backupPathKey="_data_pgdata", savePath="/app/pgBa/192.168.50.89", targetPath="/app/pgBa/192.168.50.89/hA1_full/_data_pgdata/data", restoreType=0, shortPath=0 '\000') at restore.cpp:441
#4  0x000000000040df65 in restore_backup (pConfig=0x67bb00 <g_Config>, buff=0x7fa471275010 "", buflen=10485760) at cli.cpp:736
#5  0x0000000000407944 in main (argc=9, argv=0x7ffd037027b8) at main.cpp:379


❯ pstack 271015
process: /proc/271015/mem
thread: 0x76575bfff6c0, lwp: 271087, type: 1
#0  0x0000765769e7dbe2 in <unknown>() in /usr/lib/libc.so.6
#1  0x0000765769e71e33 in <unknown>() in /usr/lib/libc.so.6
#2  0x0000765769e724bc in <unknown>() in /usr/lib/libc.so.6
#3  0x0000765769e74c0e in pthread_cond_wait!()+333 in /usr/lib/libc.so.6
#4  0x00005ec48049f917 in dequeue_parse()+48 in xxxxxxx at xxxxxxx.cpp:147
#5  0x00005ec4804a20ea in FsKernelSync::DoParserLogDispatch()+107 in xxxxxxx at xxxxxxx.cpp:770
#6  0x00005ec4804a21ea in FsKernelSync::ParserLogDispatchThread()+31 in xxxxxxx at xxxxxxx.cpp:796
#7  0x0000765769e7570a in <unknown>() in /usr/lib/libc.so.6
#8  0x0000765769ef9aac in <unknown>() in /usr/lib/libc.so.6

thread: 0x7657608ae6c0, lwp: 271086, type: 1
#0  0x0000765769e7dbe2 in <unknown>() in /usr/lib/libc.so.6
#1  0x0000765769e71e33 in <unknown>() in /usr/lib/libc.so.6
#2  0x0000765769e724bc in <unknown>() in /usr/lib/libc.so.6
#3  0x0000765769e74c0e in pthread_cond_wait!()+333 in /usr/lib/libc.so.6
#4  0x00005ec48049f79a in dequeue_download()+48 in xxxxxxx at xxxxxxx.cpp:116
#5  0x00005ec4804a1a4a in FsKernelSync::DownloadLogDispatch()+107 in xxxxxxx at xxxxxxx.cpp:622
#6  0x00005ec4804a1b32 in FsKernelSync::DownloadLogDispatchThread()+31 in xxxxxxx at xxxxxxx.cpp:645
#7  0x0000765769e7570a in <unknown>() in /usr/lib/libc.so.6
#8  0x0000765769ef9aac in <unknown>() in /usr/lib/libc.so.6

thread: 0x765768ddd6c0, lwp: 271085, type: 1
#0  0x0000765769e7dbe2 in <unknown>() in /usr/lib/libc.so.6
#1  0x0000765769e71e33 in <unknown>() in /usr/lib/libc.so.6
#2  0x0000765769e724bc in <unknown>() in /usr/lib/libc.so.6
#3  0x0000765769e74e11 in pthread_cond_timedwait!()+368 in /usr/lib/libc.so.6
#4  0x00005ec48049f5ed in dequeue_sync_timeout()+247 in xxxxxxx at xxxxxxx.cpp:80
#5  0x00005ec4804a161d in FsKernelSync::KernelStatSyncDispatch()+142 in xxxxxxx at xxxxxxx.cpp:551
#6  0x00005ec4804a16b6 in FsKernelSync::KernelStatSyncDispatchThread()+31 in xxxxxxx at xxxxxxx.cpp:569
#7  0x0000765769e7570a in <unknown>() in /usr/lib/libc.so.6
#8  0x0000765769ef9aac in <unknown>() in /usr/lib/libc.so.6

thread: 0x7657695de6c0, lwp: 271015, type: 1
#0  0x0000765769e7dbe2 in <unknown>() in /usr/lib/libc.so.6
#1  0x0000765769e71e33 in <unknown>() in /usr/lib/libc.so.6
#2  0x0000765769e71e74 in <unknown>() in /usr/lib/libc.so.6
#3  0x0000765769eec53e in poll!()+29 in /usr/lib/libc.so.6
#4  0x00005ec4804a369c in rpc_read_is_ready()+95 in xxxxxxx at rpc_io.h:23
#5  0x00005ec4804a60fd in FSDeamon::RunLoop()+254 in xxxxxxx at fsdeamon.cpp:455
#6  0x00005ec480463238 in ProcessHelper::ForkProcess()+2125 in xxxxxxx at process_helper.cpp:273
#7  0x00005ec480462515 in ProcessHelper::AddKernelProcess()+216 in xxxxxxx at process_helper.cpp:93
#8  0x00005ec48048df26 in AddSource()+1008 in xxxxxxx at fs_service.cpp:263
#9  0x00005ec48048cc73 in FsService::CLIService()+796 in xxxxxxx at fs_service.cpp:81
#10 0x00005ec48048d984 in FsService::Proccess()+127 in xxxxxxx at fs_service.cpp:194
#11 0x0000765769e7570a in <unknown>() in /usr/lib/libc.so.6
#12 0x0000765769ef9aac in <unknown>() in /usr/lib/libc.so.6
```
