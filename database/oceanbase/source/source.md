# source


## 恢复备份存储层实现
```shell
deps/oblib/src/lib/restore/ob_storage.cpp:ObStorageUtil
    deps/oblib/src/lib/restore/ob_i_storage.h:ObIStorageUtil (抽象接口层)

deps/oblib/src/lib/restore/ob_storage.cpp:ObStorageUtil::get_util (获取存储抽象接口实现)


```

- 追加实现
```shell
deps/oblib/src/lib/restore/ob_storage.cpp:ObStorageAppender
```

- 存储抽象接口
```cpp
ObIStorageUtil::
virtual int is_exist(const common::ObString& uri, const common::ObString& storage_info, bool& exist) = 0;
virtual int get_file_length(
  const common::ObString& uri, const common::ObString& storage_info, int64_t& file_length) = 0;
virtual int del_file(const common::ObString& uri, const common::ObString& storage_info) = 0;
virtual int write_single_file(
  const common::ObString& uri, const common::ObString& storage_info, const char* buf, const int64_t size) = 0;
virtual int mkdir(const common::ObString& uri, const common::ObString& storage_info) = 0;
virtual int update_file_modify_time(const common::ObString& uri, const common::ObString& storage_info) = 0;
virtual int list_files(const common::ObString& dir_path, const common::ObString& storage_info,
  common::ObIAllocator& allocator, common::ObIArray<common::ObString>& file_names) = 0;
virtual int del_dir(const common::ObString& uri, const common::ObString& storage_info) = 0;
virtual int get_pkeys_from_dir(const common::ObString& dir_path, const common::ObString& storage_info,
  common::ObIArray<common::ObPartitionKey>& pkeys) = 0;
virtual int delete_tmp_files(const common::ObString& dir_path, const common::ObString& storage_info) = 0;
virtual int is_empty_directory(
  const common::ObString& uri, const common::ObString& storage_info, bool& is_empty_directory) = 0;
virtual int is_tagging(const common::ObString& uri, const common::ObString& storage_info, bool& is_tagging) = 0;
virtual int check_backup_dest_lifecycle(
  const common::ObString& path, const common::ObString& storage_info, bool& is_set_lifecycle) = 0;
virtual int list_directories(const common::ObString& dir_path, const common::ObString& storage_info,
  common::ObIAllocator& allocator, common::ObIArray<common::ObString>& directory_names) = 0;

ObStorageAppender:: 
int open(const common::ObString& uri, const common::ObString& storage_info, const AppenderParam& param);

// TODO: out of date interface, to be deprecated.
int open_deprecated(const common::ObString& uri, const common::ObString& storage_info);
int write(const char* buf, const int64_t size);
int pwrite(const char* buf, const int64_t size, const int64_t offset);
int close();
bool is_opened() const
{
return is_opened_;
}
int64_t get_length();
```

### oss
```shell
deps/oblib/src/lib/restore/ob_storage_oss_base.h:ObStorageOssUtil

deps/oblib/src/lib/restore/ob_storage_oss_base.h:ObStorageOssAppendWriter
```

## archive

```shelll
src/archive/ob_archive_sender.cpp:ObArchiveSender::handle
    src/archive/ob_archive_sender.cpp:ObArchiveSender::archive_log_
        src/archive/ob_archive_sender.cpp:ObArchiveSender::do_archive_log_
```


## restore
```shell
ObRestoreScheduler::process_restore_job
```
