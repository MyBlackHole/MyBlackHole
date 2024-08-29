# backup

```shell
#0  oceanbase::rootserver::ObBackupSetTaskMgr::backup_user_meta_ (this=0x748cb394dda8) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:499
#1  0x000064cc6c92fd0b in oceanbase::rootserver::ObBackupSetTaskMgr::process (this=0x748cb394dda8) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:157
#2  0x000064cc6c92ada8 in oceanbase::rootserver::ObUserTenantBackupJobMgr::do_set_task_ (this=0x748d193a5f70) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1387
#3  0x000064cc6c929eaa in oceanbase::rootserver::ObUserTenantBackupJobMgr::process (this=0x748d193a5f70) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1145
#4  0x000064cc6c8d854b in oceanbase::rootserver::ObBackupDataScheduler::process (this=0x748cb8be29d8) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:972
#5  0x000064cc6c8d7f82 in oceanbase::rootserver::ObBackupDataService::process (this=0x748cb8be28f0, last_schedule_ts=@0x748cb394f448: 1724919491973398) at ./src/rootserver/backup/ob_backup_service.cpp:162
#6  0x000064cc6c8d72bc in oceanbase::rootserver::ObBackupService::run2 (this=0x748cb8be28f0) at ./src/rootserver/backup/ob_backup_service.cpp:89
#7  0x000064cc6c891709 in oceanbase::rootserver::ObBackupBaseService::run1 (this=0x748cb8be28f0) at ./src/rootserver/backup/ob_backup_base_service.cpp:44
#8  0x000064cc782a0ef9 in oceanbase::lib::MyReentrantThread::run2 (this=0x748cb90df380) at ./deps/oblib/src/lib/thread/thread_mgr.h:221
#9  0x000064cc79ac084a in oceanbase::share::ObReentrantThread::blocking_run (this=0x748cb90df380) at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:191
#10 0x000064cc782a0f18 in oceanbase::lib::MyReentrantThread::blocking_run (this=0x748cb90df380) at ./deps/oblib/src/lib/thread/thread_mgr.h:223
#11 0x000064cc79ac0192 in oceanbase::share::ObReentrantThread::run1 (this=0x748cb90df380) at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:161
#12 0x000064cc79accb93 in oceanbase::lib::Threads::run (this=0x748cb90df380, idx=0) at ./deps/oblib/src/lib/thread/threads.cpp:205
#13 0x000064cc79ac95a1 in oceanbase::lib::Thread::run (this=0x748cb4d5abc0) at ./deps/oblib/src/lib/thread/thread.cpp:164
#14 0x000064cc79ac90de in oceanbase::lib::Thread::__th_start (arg=0x748cb4d5abc0) at ./deps/oblib/src/lib/thread/thread.cpp:322
#15 0x0000748d1b65739d in ?? () from /usr/lib/libc.so.6
#16 0x0000748d1b6dc49c in ?? () from /usr/lib/libc.so.6
```

- backup dag
```shell
ob_backup_data_set_task_mgr.cpp::ObBackupSetTaskMgr::process
```
