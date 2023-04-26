# rman
oracle 备份工具

## 命令
### configure

1. configure retention policy to redundancy 1；
    舍弃备份原则，共三种，分别是：
    1. CONFIGURE RETENTION POLICY TO RECOVERY WINDOW OF 7 DAYS;
        保持所有足够的备份，可以将数据库系统恢复到最近七天内的任意时刻。任何超过最近七天的数据库备份将被标记为obsolete。
    2. CONFIGURE RETENTION POLICY TO REDUNDANCY 5;
        保持可以恢复的最新的5份数据库备份，任何超过最新5份的备份都将被标记为redundancy。一般采用的方法，默认值为1，可以设置为5。
    3. CONFIGURE RETENTION POLICY TO NONE;
        不需要保持策略，clear将恢复回默认的保持策略。
　
2. CONFIGURE BACKUP OPTIMIZATION OFF；
    默认为关闭，如果打开，rman将对备份的数据文件及归档等文件进行一种优化的算法。
　
3. Configure default device type to disk；
    默认值是硬盘，是指定所有I/O操作的设备类型是硬盘或者磁带(SBT)。
　
4. CONFIGURE CONTROLFILE AUTOBACKUP OFF；
    强制数据库在备份文件或者执行改变数据库结构的命令之后将控制文件自动备份，默认值为关闭。这样在控制文件和catalog丢失后，控制文件仍然可以恢复。
　
5. CONFIGURE CONTROLFILE AUTOBACKUP FORMAT FOR DEVICE TYPE DISK TO '%F'；
    配置控制文件的备份路径和备份格式
　
6. CONFIGURE DEVICE TYPE DISK PARALLELISM 1;
    配置数据库设备类型的并行度，默认为1。
　
7. CONFIGURE DATAFILE BACKUP COPIES FOR DEVICE TYPE DISK TO 1;
    配置每次备份的copy数量。
　
8. CONFIGURE ARCHIVELOG BACKUP COPIES FOR DEVICE TYPE DISK TO 1；
    配置归档日志存放的设备类型。
　
9. configure maxsetsize 大小；
    配置备份集的最大尺寸，默认值是unlimited，单位bytes,K,M,G；
　
10. CONFIGURE SNAPSHOT CONTROLFILE NAME TO 'C:ORACLE…SNCFTEST.ORA'；
    配置控制文件的快照文件的存放路径和文件名，这个快照文件是在备份期间产生的，用于控制文件的读一致性。
　
11. CONFIGURE CHANNEL DEVICE TYPE DISK FORMAT 'C:...%d_DB_%u_%s_%p';
    配置备份文件的备份路径和备份格式。
　
12. CONFIGURE CHANNEL DISK CLEAR;
    用于清除上面的信道配置。
　
13. CONFIGURE EXCLUDE FOR TABLESPACE [CLEAR];
    不备份指定的表空间到备份集中，对只读表空间是非常有用的。
　
14. configure channel device type disk format 'e:\backupb%d_db_%u';
    将备份文件存储到e:\backupb，后面的%d_db_%u是存储格式。
　
15. configure controlfile autobackup format for device type disk to 'e:\backupcontrol%F';
    指定control file存储在另一个路径： e:\backupcontrol，后面的%F是存储格式。
　
### list 
列出备份集和数据文件镜像
1. list incarnation;
    汇总查询，多备份文件时，可以对备份文件有个总体了解。
　
2. list backup;
    列出备份详细信息。
　
3. list backup summary;
    简述可用的备份（TY: B代表备份, LV: F代表全备fullbackup, A表示 Archivelog, 0,1,2 表示备份级别, S表示状态, A表示available可用, X表示expried过期）。
    ```shell
    备份列表
    ===============
    关键字     TY LV S 设备类型 完成时间   段数 副本数 压缩标记
    ------- -- -- - ----------- ---------- ------- ------- ---------- ---
    11      B  F  A DISK        02-7月 -13 1       1       NO         TAG20130702T162726
    12      B  F  A DISK        14-2月 -14 1       1       NO         TAG20140214T140119
    13      B  F  A DISK        14-2月 -14 1       1       NO         TAG20140214T140119
    14      B  F  A DISK        21-2月 -14 1       1       NO         TAG20140221T125325
    15      B  F  A DISK        21-2月 -14 1       1       NO         TAG20140221T125325
    16      B  A  A DISK        24-2月 -14 1       1       NO         TAG20140224T125128
    ```
4. list backup by file;
    按照文件类型列出以下四种类型列表：
    数据文件备份列表. 已存档的日志备份列表. 控制文件备份列表. SPFILE 备份的列表。
　
5. list backup of database summary;
　
6. list backup of tablespace users;
　
7. list backup of archivelog all;
    查看已经备份的 archive log 情况。
　
8. list archivelog all;
    查看目前所有的archivelog文件 (可能包含已经备份的, 除非你在备份时有参数 delete input file 删除了)。
　
9. list backup of spfile;
　
10. list backup of controlfile;
　
11. list backup verbose;
　
12. list backup of datafile 1 [n | <dir>];
　
13. list backup of archivelog from sequence 1000 until sequence 1020;
　
14. list backupset tag=TAG20140317T155753;
　
15. list expried backup;
    列出过去的备份文件。

### report
显示存储仓库(Repository)中详细的分析信息
1. report schema;
    报告目标数据库的物理结构。
　
2. report need backup;
    报告需要备份的数据文件(根据条件不同)。
　
3. report need backup days 3;
    最近三天没有备份的数据文件(如果出问题的话，这些数据文件将需要最近3天的归档日志才能恢复)。
　
4. report need backup redundancy 3;
    报告出冗余次数小于3的数据文件。
　
5. report need backup recovery window of 3 days;
    报告出恢复需要3天归档日志的数据文件。
　
6. report obsolete;
    报告已经丢弃的备份(前提是设置了备份策略)。
　
7. report unrecoverable;
    报告当前数据库中不可恢复的数据文件(即没有这个数据文件的备份. 或者该数据文件的备份已经过期)。
　
8. report schema at time ‘sysdate – 7’;
　
9. report need backup days 2 tablespace system;

### delete
删除相关的备份集或镜像副本的物理文件, 同时将删除标记 delete 更新到控制文件。
```shell
delete backupset;
delete backupset n;
delete obsolete;   -- 删除荒废
delete noprompt obsolete;  -- 不提示, 删除荒废
delete noprompt expired backup;  -- 不提示, 删除不在磁盘上的备份集(可以先用crosscheck同步确认一下)
delete obsolete redundancy 2;
delete noprompt copy
delete noprompt backupset tag TAG20140317T14432;
delete obsolete recovery window of 7 days;
delete expired backupset;
delete expired copy;
delete expired archivelog all;
```

### Crosscheck
设置状态为AVAILABLE(可用)或者EXPIRED(不可用)

执行crosscheck时，RMAN检查目录中列出的每个备份集或副本并且判断他们是否存在与备份介质上。
如果备份集或副本不存在与备份介质上，它就会被标记为expired, 并且不能用于任何还原操作；
如果备份集或副本存在与备份介质上，它就会维持available状态。
如果以前被标记为expired 的备份集或副本再次存在于备份介质上，crosscheck 命令就会将它标记回available。
　
1. RMAN 备份检验的几种状态：
    expired: 对象不存在于磁盘或磁带。
    available: 对象处于可用状态。
    unavailabe: 对象处于不可用状态。
　
2. expired 与 obsolette 的区别:
    1. 对于EXPIRED状态，与crosscheck命令是密切相关的，RMAN通过crosscheck命令检查备份是否存在于备份介质上, 如果不存在，则状态由AVAILABLE改为EXPIRED。
    2. 对于obsolete状态，是针对MAN备份保留策略来说的，超过了这个保留策略的备份，会被标记为obsolete，但其状态依旧为AVAILABLE，我们可以使用report obsolete来查看已废弃的备份。
    ```shell
    rosscheck backup; --校验备份片（？？？）
    crosscheck backupset; --校验备份集
    crosscheck copy； --校验镜像副本
    crosscheck backup of controlfile; --校验备份的控制文件
    crosscheck backup of archivelog all; --校验所有备份的归档日志
    crosscheck backup of datafile 1,2; --校验datafile 1,2
    crosscheck backup of tablespace sysaux,system; --校验表空间sysaux,system
    crosscheck backup completed between '13-OCT-10' and '23-OCT-10'; --校验时间段,时间段格式由NLS_DATE_FORMAT设置
    crosscheck backupset 1067,1068; --校验指定的备份集
    ```
### backup

1. 将备份集放到快速恢复区中：
    1. 归档模式下：
        backup database plus archivelog;
    2. 非归档模式下：
        shutdown immediate;   # 关闭一致性后，打开到mount状态
        backup database
　
2. 指定备份片段的存放路径和命名规则：
    backup format '/u01/app/oracle/oradata/enmo1/AL_%d/%t/%s/%p' archivelog like '%arc_dest%';
　
3. 备份成镜像文件：
    backup as copy
　
4. 设置备份标记（每个标记必须唯一，相同的标记可以用于多个备份只还原最新的备份）。
    backup database tag='full_bak1';
　
5. 设置备份集大小（必须大于数据库总数据文件的大小，否则会报错）。
    backup database maxsetsize=100m tag='datafile1';
　
6. 设置备份片大小(磁带或文件系统限制)
    ```shell
    run {
        allocate channel c1 type disk maxpicecsize 100m format '/data/backup/full_0_%U_%T';        
        backup database tag='full_0';        
        release channel c1;        
    } 
    PS：
    可以在allocate子句中设定每个备份片的大小，以达到磁带或系统限制。
    也可以在configure中设置备份片大小。

    Configure channel device type disk maxpiecesize 100 m        
    Configure channel device type disk clear;
    ```

7. 备份集的保存策略
    backup database keep forever;  --永久保留备份文件, 这种需要有恢复目录的支持
    backup database keep until time='sysdate+30'; --保存备份30天

8. 重写configure exclude命令
    backup databas noexclude keep forever tag='test backup';
　
9. 检查数据库错误
    backup validate database; 
    使用RMAN来扫描数据库的物理/逻辑错误，并不执行实际备份。

10. 跳过脱机，不可存取或只读文件
    backup database skip readonly;
    backup database skip offline;
    backup database skip inaccessible;
    backup database ship readonly skip offline ship inaccessible;
　
11. 强制备份
    backup database force;
　
12. 基于上次备份时间备份数据文件
    1. 只备份添加的新数据文件：
        backup database not backed up;
    2. 备份"在限定时间周期内"没有被备份的数据文件：
        backup database not backed up since time='sysdate-2';
　
13. 备份操作期间检查逻辑错误
    backup check logical database;
    backup validate check logical database;
　
14. 生成备份副本
    backup database copies=2;

15. 备份控制文件
    backup database device type disk includ current controlfile;

### format
```shell
%a：Oracle数据库的activation ID即RESETLOG_ID。
%c：备份片段的复制数（从1开始编号，最大不超过256）。
%d：Oracle数据库名称。
%D：当前时间中的日，格式为DD。
%e：归档序号。
%f：绝对文件编号。
%F：基于"DBID+时间"确定的唯一名称，格式的形式为c-IIIIIIIIII-YYYYMMDD-QQ,其中IIIIIIIIII 为该数据库的DBID，YYYYMMDD为日期，QQ是一个1～256的序列。
%h：归档日志线程号。
%I：Oracle数据库的DBID。
%M：当前时间中的月，格式为MM。
%N：表空间名称。
%n：数据库名称，并且会在右侧用x字符进行填充，使其保持长度为8。
%p：备份集中备份片段的编号，从1开始。
%s：备份集号。
%t：备份集时间戳。
%T：当前时间的年月日格式（YYYYMMDD）。
%u：是一个由备份集编号和建立时间压缩后组成的8字符名称。利用%u可以为每个备份集生成一个唯一的名称。
%U：默认是%u_%p_%c的简写形式，利用它可以为每一个备份片段（即磁盘文件）生成一个唯一名称，这是最常用的命名方式。执行不同备份操作时，生成的规则也不同，如下所示：

生成备份片段时，%U=%u_%p_%c；
生成数据文件镜像复制时，%U=data-D-%d_id-%I_TS-%N_FNO-%f_%u；
生成归档文件镜像复制时，%U=arch-D_%d-id-%I_S-%e_T-%h_A-%a_%u；
生成控制文件镜像复制时，%U=cf-D_%d-id-%I_%u。
%Y：当前时间中的年，格式为YYYY。

PS：
如果在BACKUP命令中没有指定FORMAT选项，则RMAN默认使用%U为备份片段命名。
```

## 例子
- 本地连接
```shell
rman target /
```

- 远程连接
```shell
rman target sys/oracle@orcl
```

- 查看配置
```shell
show all
```

```shell
show channel; // 通道分配
show device type; // IO 设备类型
show retention policy; // 保存策略
show datafile backup copies; // 多个备份的拷贝数目
show maxsetsize; // 备份集大小的最大值
show exclude; // 不必备份的表空间
show backup optimization; // 备份的优化
```
