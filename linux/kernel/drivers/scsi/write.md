# write

用户层写入 iscsi 设备

```shell
// 1. 虚拟文件系统层
vfs_write()
  → ext4_file_write_iter()  // 假设ext4文件系统
    → generic_perform_write()

// 2. 页面缓存层
// 数据先写入页面缓存
__generic_file_write_iter()
  → iov_iter_copy_from_user_atomic()
  → mark_inode_dirty()

// 3. 块设备层
// 回写脏页时触发
ext4_writepages()
  → mpage_submit_page()
    → submit_bio()  // 创建bio请求
      → submit_bio_noacct
        → submit_bio_noacct_nocheck
            → __submit_bio_noacct_mq
                → __submit_bio
                    → blk_finish_plug
                        → ...
                            → blk_mq_dispatch_rq_list
                            <!-- ret = q->mq_ops->queue_rq(hctx, &bd); -->
                                → queue_rq

// 4. SCSI中层
queue_rq()
  → generic_make_request()
    → scsi_queue_rq()  // 进入SCSI中层
      → scsi_dispatch_cmd()
```
