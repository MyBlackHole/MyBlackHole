# debug

- 编译方式
```shell
<!--先编译好 observer-->
./tools/deploy/obd.sh prepare -p /tmp/obtest

<!--启动-->
./tools/deploy/obd.sh deploy -c ./tools/deploy/single.yaml

<!--/run/media/black/Data/Documents/github/cpp/oceanbase/tools/deploy/.obd/log/obd 密码在里面, 运行成功也会有提示-->
mysql -uroot -h127.0.0.1 -P10000 -pxxxx
./deps/3rd/u01/obclient/bin/obclient -h127.0.0.1 -P10000 -uroot -Doceanbase -A -pxxxxxx

<!--清理-->
./tools/deploy/obd.sh destroy --rm -n single
```


- 安装 rpm 方式
```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/oceanbase/OceanBase.repo
sudo yum install -y libtool libaio obclient
cd {your_oceanbase_project}/tools/deploy # 进入项目目录下的tools/deploy目录
./obd.sh prepare # 准备配置文件
./obd.sh deploy -c single.yaml # 根据配置文件，在本地部署一个ObServer


cd {your_oceanbase_project}/tools/deploy 
./obd.sh gdb

<!-- 二次启动可以 -->
bin/observer

gdb bin/observer
```

- debug-info 方式 (社区版)
[连接](https://github.com/oceanbase/oceanbase/blob/develop/docs/docs/zh/debug.md)
```shell
<!--如何查找 debug-info 包-->
[admin@dev-obv4-01-67-19 ~]$ ./oceanbase/bin/observer -V
./oceanbase/bin/observer -V
observer (OceanBase 4.2.1.6)

REVISION: 106000022024042414-e43efbffdcc50956239225104e944068375568b6
BUILD_BRANCH: HEAD
BUILD_TIME: Apr 24 2024 16:17:08
BUILD_FLAGS: RelWithDebInfo
BUILD_INFO:

Copyright (c) 2011-present OceanBase Inc.

<!--去上面连接的 rpm 网站查找 106000022024042414(这个不是社区版的)-->
```

- 连接数据库
```shell
/media/black/Data/Documents/github/Cpp/oceanbase/deps/3rd/u01/obclient/bin/obclient -h 127.0.0.1 -P 10000 -uroot
```

- GCONF
```shell
src/share/parameter/ob_parameter_seed.ipp
```

- 归档逻辑
```shell
ob_archive_sender.cpp:ObArchiveSender::handle
```

- 物理恢复
```shell
ObPhysicalRestoreTenantExecutor::execute
<!-- rpc 调用 root service -->
ObRootService::physical_restore_tenant

------

src/rootserver/restore/ob_restore_replica.cpp:ObRestoreReplica::do_restore


------
src/rootserver/restore/ob_restore_scheduler.cpp:ObRestoreScheduler::run3
    src/rootserver/restore/ob_restore_scheduler.cpp:ObRestoreScheduler::process_restore_job
        src/rootserver/restore/ob_restore_scheduler.cpp:ObRestoreScheduler::restore_tenant
            src/rootserver/restore/ob_restore_scheduler.cpp:ObRestoreScheduler::fill_job_info
                src/rootserver/restore/ob_restore_scheduler.cpp:ObRestoreScheduler::fill_backup_info


------ 

src/share/backup/ob_backup_info_mgr.cpp:ObRestoreBackupInfoUtil::get_restore_backup_info
    src/share/backup/ob_backup_info_mgr.cpp:ObRestoreBackupInfoUtil::get_restore_backup_info_v1_
    src/share/backup/ob_backup_info_mgr.cpp:ObRestoreBackupInfoUtil::get_restore_backup_info_v2_

<!-- 恢复时间检查 -->
src/share/backup/ob_multi_backup_dest_util.cpp:ObMultiBackupDestUtil::get_compat_backup_piece_list
    src/share/backup/ob_log_archive_backup_info_mgr.cpp:ObExternLogArchiveBackupInfo::get_log_archive_status
```

