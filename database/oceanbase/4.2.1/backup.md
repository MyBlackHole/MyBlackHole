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

- major_data_info_turn_1
```shell
Thread 153 "T1002_PREFETCH_" hit Breakpoint 2.2, oceanbase::share::ObBackupPathUtil::get_ls_info_data_info_dir_path (
    backup_tenant_dest=..., desc=..., type=..., turn_id=1, backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1043
1043      int ret = OB_SUCCESS;
(gdb) bt
#0  oceanbase::share::ObBackupPathUtil::get_ls_info_data_info_dir_path (backup_tenant_dest=..., desc=..., type=..., turn_id=1,
    backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1043
#1  0x000058f201c742e5 in oceanbase::backup::ObBackupTenantIndexRetryIDGetter::get_ls_info_data_info_dir_path_ (this=0x71ed2afcb840,
    backup_path=...) at ./src/storage/backup/ob_backup_index_store.cpp:1481
#2  0x000058f201c3c7fd in oceanbase::backup::ObBackupTenantIndexRetryIDGetter::get_max_retry_id (this=0x71ed2afcb840,
    retry_id=@0x71ed2afcb9f0: 0) at ./src/storage/backup/ob_backup_index_store.cpp:1428
#3  0x000058f201d29012 in oceanbase::backup::ObBackupTabletProvider::get_tenant_meta_index_retry_id_ (this=0x71ecfddf2030,
    backup_data_type=..., turn_id=1, retry_id=@0x71ed2afcb9f0: 0) at ./src/storage/backup/ob_backup_utils.cpp:2369
#4  0x000058f201d27abd in oceanbase::backup::ObBackupTabletProvider::build_tenant_meta_index_store_ (this=0x71ecfddf2030,
    backup_data_type=...) at ./src/storage/backup/ob_backup_utils.cpp:2331
#5  0x000058f201d1cf9d in oceanbase::backup::ObBackupTabletProvider::check_tablet_continuity_ (this=0x71ecfddf2030, ls_id=...,
    tablet_id=..., tablet_handle=...) at ./src/storage/backup/ob_backup_utils.cpp:2274
#6  0x000058f201d1979b in oceanbase::backup::ObBackupTabletProvider::prepare_tablet_ (this=0x71ecfddf2030, tenant_id=1002, ls_id=...,
    tablet_id=..., backup_data_type=..., total_count=@0x71ed2afccbe8: 0) at ./src/storage/backup/ob_backup_utils.cpp:1719
#7  0x000058f201d16ab5 in oceanbase::backup::ObBackupTabletProvider::prepare_batch_tablet_ (this=0x71ecfddf2030, tenant_id=1002,
    ls_id=...) at ./src/storage/backup/ob_backup_utils.cpp:1666
#8  0x000058f201d160ba in oceanbase::backup::ObBackupTabletProvider::get_next_batch_items (this=0x71ecfddf2030, items=...,
    task_id=@0x71ed2afcd1b8: 0) at ./src/storage/backup/ob_backup_utils.cpp:1561
#9  0x000058f201cb2879 in oceanbase::backup::ObPrefetchBackupInfoTask::inner_process_ (this=0x71ecfcbd0ce0)
    at ./src/storage/backup/ob_backup_task.cpp:2149
#10 0x000058f201cb2135 in oceanbase::backup::ObPrefetchBackupInfoTask::process (this=0x71ecfcbd0ce0)
    at ./src/storage/backup/ob_backup_task.cpp:2024
#11 0x000058f2041841e9 in oceanbase::share::ObITask::do_work (this=0x71ecfcbd0ce0) at ./src/share/scheduler/ob_dag_scheduler.cpp:241
#12 0x000058f1f6bd239a in oceanbase::share::ObTenantDagWorker::run1 (this=0x71ed4ffb1f00)
    at ./src/share/scheduler/ob_dag_scheduler.cpp:1476
#13 0x000058f20468c393 in oceanbase::lib::MyThreadPool::run1 (this=0x71ed732dbf90) at ./deps/oblib/src/lib/thread/thread_mgr.h:336
#14 0x000058f205ebeb93 in oceanbase::lib::Threads::run (this=0x71ed732dbf90, idx=0) at ./deps/oblib/src/lib/thread/threads.cpp:205
#15 0x000058f205ebb5a1 in oceanbase::lib::Thread::run (this=0x71ed3edff6d0) at ./deps/oblib/src/lib/thread/thread.cpp:164
#16 0x000058f205ebb0de in oceanbase::lib::Thread::__th_start (arg=0x71ed3edff6d0) at ./deps/oblib/src/lib/thread/thread.cpp:322
#17 0x000071ed8610e39d in ?? () from /usr/lib/libc.so.6
#18 0x000071ed8619349c in ?? () from /usr/lib/libc.so.6
```

- infos/meta_info 
```shell
#0  oceanbase::share::ObBackupPathUtil::get_tenant_meta_info_dir_path (backup_set_dest=..., backup_path=...)
    at ./src/share/backup/ob_backup_path.cpp:1057
#1  0x000058f203fea9c0 in oceanbase::share::ObBackupPathUtil::get_backup_ls_attr_info_path (backup_set_dest=..., turn_id=1,
    backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1085
#2  0x000058f201bffd00 in oceanbase::storage::ObBackupDataStore::write_ls_attr (this=0x71ed4574eb40, turn_id=1, ls_info=...)
    at ./src/storage/backup/ob_backup_data_store.cpp:347
#3  0x000058f1f8d32830 in oceanbase::rootserver::ObBackupSetTaskMgr::persist_ls_attr_info_ (this=0x71ed4574dda8, sys_ls_task=...,
    ls_ids=...) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:293
#4  0x000058f1f8d2d272 in oceanbase::rootserver::ObBackupSetTaskMgr::backup_sys_meta_ (this=0x71ed4574dda8)
    at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:463
#5  0x000058f1f8d21caf in oceanbase::rootserver::ObBackupSetTaskMgr::process (this=0x71ed4574dda8)
    at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:151
#6  0x000058f1f8d1cda8 in oceanbase::rootserver::ObUserTenantBackupJobMgr::do_set_task_ (this=0x71ed19409f70)
    at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1387
#7  0x000058f1f8d1beaa in oceanbase::rootserver::ObUserTenantBackupJobMgr::process (this=0x71ed19409f70)
    at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1145
#8  0x000058f1f8cca54b in oceanbase::rootserver::ObBackupDataScheduler::process (this=0x71ed4e3e29d8)
    at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:972
#9  0x000058f1f8cc9f82 in oceanbase::rootserver::ObBackupDataService::process (this=0x71ed4e3e28f0,
    last_schedule_ts=@0x71ed4574f448: 1724986078579391) at ./src/rootserver/backup/ob_backup_service.cpp:162
#10 0x000058f1f8cc92bc in oceanbase::rootserver::ObBackupService::run2 (this=0x71ed4e3e28f0)
    at ./src/rootserver/backup/ob_backup_service.cpp:89
#11 0x000058f1f8c83709 in oceanbase::rootserver::ObBackupBaseService::run1 (this=0x71ed4e3e28f0)
    at ./src/rootserver/backup/ob_backup_base_service.cpp:44
#12 0x000058f204692ef9 in oceanbase::lib::MyReentrantThread::run2 (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/thread_mgr.h:221
#13 0x000058f205eb284a in oceanbase::share::ObReentrantThread::blocking_run (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:191
#14 0x000058f204692f18 in oceanbase::lib::MyReentrantThread::blocking_run (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/thread_mgr.h:223
#15 0x000058f205eb2192 in oceanbase::share::ObReentrantThread::run1 (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:161
#16 0x000058f205ebeb93 in oceanbase::lib::Threads::run (this=0x71ed60989d10, idx=0) at ./deps/oblib/src/lib/thread/threads.cpp:205
#17 0x000058f205ebb5a1 in oceanbase::lib::Thread::run (this=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:164
#18 0x000058f205ebb0de in oceanbase::lib::Thread::__th_start (arg=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:322
--Type <RET> for more, q to quit, c to continue without paging--
#19 0x000071ed8610e39d in ?? () from /usr/lib/libc.so.6
#20 0x000071ed8619349c in ?? () from /usr/lib/libc.so.6


#0  oceanbase::share::ObBackupPathUtil::get_tenant_meta_info_dir_path (backup_set_dest=..., backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1057
#1  0x000058f203feae4c in oceanbase::share::ObBackupPathUtil::get_ls_meta_infos_path (backup_set_dest=..., backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1100
#2  0x000058f201c01488 in oceanbase::storage::ObBackupDataStore::write_ls_meta_infos (this=0x71ed4574eb40, ls_meta_infos=...) at ./src/storage/backup/ob_backup_data_store.cpp:405
#3  0x000058f1f8d383ac in oceanbase::rootserver::ObBackupSetTaskMgr::merge_ls_meta_infos_ (this=0x71ed4574dda8, ls_tasks=...) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:735
#4  0x000058f1f8d2e628 in oceanbase::rootserver::ObBackupSetTaskMgr::backup_meta_finish_ (this=0x71ed4574dda8) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:577
#5  0x000058f1f8d21d67 in oceanbase::rootserver::ObBackupSetTaskMgr::process (this=0x71ed4574dda8) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:163
#6  0x000058f1f8d1cda8 in oceanbase::rootserver::ObUserTenantBackupJobMgr::do_set_task_ (this=0x71ecf2483f70) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1387
#7  0x000058f1f8d1beaa in oceanbase::rootserver::ObUserTenantBackupJobMgr::process (this=0x71ecf2483f70) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1145
#8  0x000058f1f8cca54b in oceanbase::rootserver::ObBackupDataScheduler::process (this=0x71ed4e3e29d8) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:972
#9  0x000058f1f8cc9f82 in oceanbase::rootserver::ObBackupDataService::process (this=0x71ed4e3e28f0, last_schedule_ts=@0x71ed4574f448: 1724986255316717) at ./src/rootserver/backup/ob_backup_service.cpp:162
#10 0x000058f1f8cc92bc in oceanbase::rootserver::ObBackupService::run2 (this=0x71ed4e3e28f0) at ./src/rootserver/backup/ob_backup_service.cpp:89
#11 0x000058f1f8c83709 in oceanbase::rootserver::ObBackupBaseService::run1 (this=0x71ed4e3e28f0) at ./src/rootserver/backup/ob_backup_base_service.cpp:44
#12 0x000058f204692ef9 in oceanbase::lib::MyReentrantThread::run2 (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/thread_mgr.h:221
#13 0x000058f205eb284a in oceanbase::share::ObReentrantThread::blocking_run (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:191
#14 0x000058f204692f18 in oceanbase::lib::MyReentrantThread::blocking_run (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/thread_mgr.h:223
#15 0x000058f205eb2192 in oceanbase::share::ObReentrantThread::run1 (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:161
#16 0x000058f205ebeb93 in oceanbase::lib::Threads::run (this=0x71ed60989d10, idx=0) at ./deps/oblib/src/lib/thread/threads.cpp:205
#17 0x000058f205ebb5a1 in oceanbase::lib::Thread::run (this=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:164
#18 0x000058f205ebb0de in oceanbase::lib::Thread::__th_start (arg=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:322
#19 0x000071ed8610e39d in ?? () from /usr/lib/libc.so.6
#20 0x000071ed8619349c in ?? () from /usr/lib/libc.so.6


```

- infos 备份逻辑
```shell
ob_backup_task.cpp::ObPrefetchBackupInfoTask::inner_process_()
```




```shell

#0  oceanbase::share::ObBackupPathUtil::get_tenant_meta_info_dir_path (backup_set_dest=..., backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1057
#1  0x000058f203feae4c in oceanbase::share::ObBackupPathUtil::get_ls_meta_infos_path (backup_set_dest=..., backup_path=...) at ./src/share/backup/ob_backup_path.cpp:1100
#2  0x000058f201c01488 in oceanbase::storage::ObBackupDataStore::write_ls_meta_infos (this=0x71ed4574eb40, ls_meta_infos=...) at ./src/storage/backup/ob_backup_data_store.cpp:405
#3  0x000058f1f8d383ac in oceanbase::rootserver::ObBackupSetTaskMgr::merge_ls_meta_infos_ (this=0x71ed4574dda8, ls_tasks=...) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:735
#4  0x000058f1f8d2e628 in oceanbase::rootserver::ObBackupSetTaskMgr::backup_meta_finish_ (this=0x71ed4574dda8) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:577
#5  0x000058f1f8d21d67 in oceanbase::rootserver::ObBackupSetTaskMgr::process (this=0x71ed4574dda8) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:163
#6  0x000058f1f8d1cda8 in oceanbase::rootserver::ObUserTenantBackupJobMgr::do_set_task_ (this=0x71ed0e355fa0) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1387
#7  0x000058f1f8d1beaa in oceanbase::rootserver::ObUserTenantBackupJobMgr::process (this=0x71ed0e355fa0) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1145
#8  0x000058f1f8cca54b in oceanbase::rootserver::ObBackupDataScheduler::process (this=0x71ed4e3e29d8) at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:972
#9  0x000058f1f8cc9f82 in oceanbase::rootserver::ObBackupDataService::process (this=0x71ed4e3e28f0, last_schedule_ts=@0x71ed4574f448: 1724986328595607) at ./src/rootserver/backup/ob_backup_service.cpp:162
#10 0x000058f1f8cc92bc in oceanbase::rootserver::ObBackupService::run2 (this=0x71ed4e3e28f0) at ./src/rootserver/backup/ob_backup_service.cpp:89
#11 0x000058f1f8c83709 in oceanbase::rootserver::ObBackupBaseService::run1 (this=0x71ed4e3e28f0) at ./src/rootserver/backup/ob_backup_base_service.cpp:44
#12 0x000058f204692ef9 in oceanbase::lib::MyReentrantThread::run2 (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/thread_mgr.h:221
#13 0x000058f205eb284a in oceanbase::share::ObReentrantThread::blocking_run (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:191
#14 0x000058f204692f18 in oceanbase::lib::MyReentrantThread::blocking_run (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/thread_mgr.h:223
#15 0x000058f205eb2192 in oceanbase::share::ObReentrantThread::run1 (this=0x71ed60989d10) at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:161
#16 0x000058f205ebeb93 in oceanbase::lib::Threads::run (this=0x71ed60989d10, idx=0) at ./deps/oblib/src/lib/thread/threads.cpp:205
#17 0x000058f205ebb5a1 in oceanbase::lib::Thread::run (this=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:164
#18 0x000058f205ebb0de in oceanbase::lib::Thread::__th_start (arg=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:322
#19 0x000071ed8610e39d in ?? () from /usr/lib/libc.so.6
#20 0x000071ed8619349c in ?? () from /usr/lib/libc.so.6


#0  oceanbase::share::ObBackupPathUtil::get_backup_data_tablet_ls_info_path (backup_set_dest=..., backup_data_type=..., turn_id=1,
    path=...) at ./src/share/backup/ob_backup_path.cpp:1307
#1  0x000058f201c032a2 in oceanbase::storage::ObBackupDataStore::read_tablet_to_ls_info (this=0x71ed4574eb40, turn_id=1, type=...,
    tablet_to_ls_info=...) at ./src/storage/backup/ob_backup_data_store.cpp:501
#2  0x000058f1f8d3c0f3 in oceanbase::rootserver::ObBackupSetTaskMgr::get_tablet_list_by_snapshot (this=0x71ed4574dda8,
    consistent_scn=..., latest_ls_tablet_map=...) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:819
#3  0x000058f1f8d38b43 in oceanbase::rootserver::ObBackupSetTaskMgr::merge_tablet_to_ls_info_ (this=0x71ed4574dda8,
    consistent_scn=..., ls_tasks=..., ls_ids=...) at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:771
#4  0x000058f1f8d2e6b7 in oceanbase::rootserver::ObBackupSetTaskMgr::backup_meta_finish_ (this=0x71ed4574dda8)
    at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:579
#5  0x000058f1f8d21d67 in oceanbase::rootserver::ObBackupSetTaskMgr::process (this=0x71ed4574dda8)
    at ./src/rootserver/backup/ob_backup_data_set_task_mgr.cpp:163
#6  0x000058f1f8d1cda8 in oceanbase::rootserver::ObUserTenantBackupJobMgr::do_set_task_ (this=0x71ed1256ff70)
    at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1387
#7  0x000058f1f8d1beaa in oceanbase::rootserver::ObUserTenantBackupJobMgr::process (this=0x71ed1256ff70)
    at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:1145
#8  0x000058f1f8cca54b in oceanbase::rootserver::ObBackupDataScheduler::process (this=0x71ed4e3e29d8)
    at ./src/rootserver/backup/ob_backup_data_scheduler.cpp:972
#9  0x000058f1f8cc9f82 in oceanbase::rootserver::ObBackupDataService::process (this=0x71ed4e3e28f0,
    last_schedule_ts=@0x71ed4574f448: 1724986574765880) at ./src/rootserver/backup/ob_backup_service.cpp:162
#10 0x000058f1f8cc92bc in oceanbase::rootserver::ObBackupService::run2 (this=0x71ed4e3e28f0)
    at ./src/rootserver/backup/ob_backup_service.cpp:89
#11 0x000058f1f8c83709 in oceanbase::rootserver::ObBackupBaseService::run1 (this=0x71ed4e3e28f0)
    at ./src/rootserver/backup/ob_backup_base_service.cpp:44
#12 0x000058f204692ef9 in oceanbase::lib::MyReentrantThread::run2 (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/thread_mgr.h:221
#13 0x000058f205eb284a in oceanbase::share::ObReentrantThread::blocking_run (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:191
#14 0x000058f204692f18 in oceanbase::lib::MyReentrantThread::blocking_run (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/thread_mgr.h:223
#15 0x000058f205eb2192 in oceanbase::share::ObReentrantThread::run1 (this=0x71ed60989d10)
    at ./deps/oblib/src/lib/thread/ob_reentrant_thread.cpp:161
#16 0x000058f205ebeb93 in oceanbase::lib::Threads::run (this=0x71ed60989d10, idx=0) at ./deps/oblib/src/lib/thread/threads.cpp:205
#17 0x000058f205ebb5a1 in oceanbase::lib::Thread::run (this=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:164
#18 0x000058f205ebb0de in oceanbase::lib::Thread::__th_start (arg=0x71ed4aff1f70) at ./deps/oblib/src/lib/thread/thread.cpp:322
--Type <RET> for more, q to quit, c to continue without paging--
#19 0x000071ed8610e39d in ?? () from /usr/lib/libc.so.6
#20 0x000071ed8619349c in ?? () from /usr/lib/libc.so.6
```
