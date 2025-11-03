# debug

- FUSEMOUNT
```shell

#0  __bch2_btree_node_write (c=c@entry=0x7f04a1b8a000, b=b@entry=0x55e267d77300, flags=flags@entry=8) at libbcachefs/btree_io.c:2313
#1  0x000055e232af1263 in bch2_btree_node_write_trans (trans=0x7f0480005000, b=0x55e267d77300, lock_type_held=SIX_LOCK_read, flags=8) at libbcachefs/btree_io.c:2677
#2  0x000055e232b0bfde in btree_node_write_if_need (trans=0x7f0480005000, b=0x55e267d77300, lock_held=SIX_LOCK_read) at libbcachefs/btree_io.h:167
#3  __btree_node_flush (j=<optimized out>, pin=0x55e267d77400, i=<optimized out>, seq=20) at libbcachefs/btree_trans_commit.c:258
#4  0x000055e232b0c04e in bch2_btree_node_flush0 (j=<optimized out>, pin=<optimized out>, seq=<optimized out>) at libbcachefs/btree_trans_commit.c:265
#5  0x000055e232b96f18 in journal_flush_pins (j=j@entry=0x7f04a1bc9bc0, seq_to_flush=seq_to_flush@entry=0, allowed_below_seq=allowed_below_seq@entry=4294967295, min_any=0, min_any@entry=1,
    min_key_cache=min_key_cache@entry=0, allowed_above_seq=0) at libbcachefs/journal_reclaim.c:607
#6  0x000055e232b98645 in __bch2_journal_reclaim (j=j@entry=0x7f04a1bc9bc0, direct=direct@entry=false, kicked=<optimized out>) at libbcachefs/journal_reclaim.c:735
#7  0x000055e232b9883d in bch2_journal_reclaim_thread (arg=0x7f04a1bc9bc0) at libbcachefs/journal_reclaim.c:777
#8  0x000055e232bf7bb3 in kthread_start_fn (data=0x55e267f65680) at linux/kthread.c:25
#9  0x00007f04a18969cb in start_thread (arg=<optimized out>) at pthread_create.c:448
#10 0x00007f04a191aa0c in __GI___clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:78

#0  sync_write (bio=0x7f0480000ee8, iov=0x7f04a12ad6f0, i=1) at linux/blkdev.c:267
#1  0x000055e232bf5d73 in generic_make_request (bio=<optimized out>) at linux/blkdev.c:82
#2  0x000055e232b83a64 in submit_bio (bio=<optimized out>) at include/linux/blkdev.h:42
#3  bch2_submit_wbio_replicas (wbio=0x7f0480000ea8, c=0x7f04a1b8a000, type=type@entry=BCH_DATA_btree, k=k@entry=0x7f04a12ad810, nocow=nocow@entry=false) at libbcachefs/io_write.c:521
#4  0x000055e232ae6f63 in btree_write_submit (work=<optimized out>) at libbcachefs/btree_io.c:2308
#5  0x000055e232bfeb3b in worker_thread (arg=0x55e267d6ef00) at linux/workqueue.c:262
#6  0x000055e232bf7bb3 in kthread_start_fn (data=0x55e267d6e010) at linux/kthread.c:25
#7  0x00007f04a18969cb in start_thread (arg=<optimized out>) at pthread_create.c:448
#8  0x00007f04a191aa0c in __GI___clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:78


#0  sync_read (bio=0x55e267d68d00, iov=0x7ffe75f78890, i=1) at linux/blkdev.c:260
#1  0x000055e232bf5cb4 in generic_make_request (bio=bio@entry=0x55e267d68d00) at linux/blkdev.c:79
#2  0x000055e232be3186 in submit_bio (bio=0x55e267d68d00) at include/linux/blkdev.h:42
#3  read_back_super (c=0x7f04a1b8a000, ca=0x55e267e50800) at libbcachefs/super-io.c:987
#4  bch2_write_super (c=c@entry=0x7f04a1b8a000) at libbcachefs/super-io.c:1124
#5  0x000055e232be4569 in __bch2_check_set_feature (c=0x7f04a1b8a000, feat=feat@entry=8) at libbcachefs/super-io.c:1230
#6  0x000055e232b86ee9 in bch2_check_set_feature (feat=8, c=<optimized out>) at libbcachefs/super-io.h:106
#7  bch2_write_data_inline (op=0x7ffe75f78be0, data_len=<optimized out>) at libbcachefs/io_write.c:1618
#8  bch2_write (ws=0x7ffe75f78be0) at libbcachefs/io_write.c:1715
#9  0x000055e232aa075b in closure_queue (cl=0x7ffe75f78be0) at include/linux/closure.h:270
#10 closure_call (cl=0x7ffe75f78be0, fn=<optimized out>, wq=0x0, parent=0x0) at include/linux/closure.h:432
#11 write_aligned (c=c@entry=0x7f04a1b8a000, inum=..., io_opts=..., buf=buf@entry=0x55e267e40000, aligned_size=aligned_size@entry=512, aligned_offset=aligned_offset@entry=0, new_i_size=<optimized out>,
    written_out=<optimized out>) at c_src/cmd_fusemount.c:604
#12 0x000055e232aa20d7 in bcachefs_fuse_write (req=0x55e267e35f00, ino=<optimized out>, buf=0x7f049feb6000 "skfjsl\n", size=7, offset=<optimized out>, fi=<optimized out>) at c_src/cmd_fusemount.c:670
#13 0x00007f04a1c1064e in do_write (req=<optimized out>, nodeid=<optimized out>, inarg=<optimized out>) at ../lib/fuse_lowlevel.c:1457
#14 0x00007f04a1c1f19b in fuse_session_process_buf_internal (se=0x55e267f65770, buf=0x7ffe75f79360, ch=<optimized out>) at ../lib/fuse_lowlevel.c:2909
#15 0x00007f04a1c1681d in fuse_session_loop (se=se@entry=0x55e267f65770) at ../lib/fuse_loop.c:34
#16 0x000055e232aa3108 in cmd_fusemount (argc=<optimized out>, argv=<optimized out>) at c_src/cmd_fusemount.c:1273
#17 0x000055e232a58547 in bcachefs::handle_c_command (argv=..., symlink_cmd=...) at src/bcachefs.rs:74
#18 0x000055e232a58bcd in bcachefs::main () at src/bcachefs.rs:122


#0  sync_write (bio=0x55e267e67018, iov=0x7f04a0703790, i=1) at linux/blkdev.c:267
#1  0x000055e232bf5d73 in generic_make_request (bio=bio@entry=0x55e267e67018) at linux/blkdev.c:82
#2  0x000055e232b8dd1b in submit_bio (bio=0x55e267e67018) at include/linux/blkdev.h:42
#3  journal_write_submit (ws=0x7f04a1bc9d30) at libbcachefs/journal_io.c:1921
#4  0x000055e232bfeb3b in worker_thread (arg=0x55e267e34600) at linux/workqueue.c:262
#5  0x000055e232bf7bb3 in kthread_start_fn (data=0x55e267e34650) at linux/kthread.c:25
#6  0x00007f04a18969cb in start_thread (arg=<optimized out>) at pthread_create.c:448
#7  0x00007f04a191aa0c in __GI___clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:78


#0  __bch2_btree_node_write (c=c@entry=0x7f04a1b8a000, b=0x55e267d69a00, flags=0) at libbcachefs/btree_io.c:2313
#1  0x000055e232af12a2 in bch2_btree_node_write_trans (trans=0x55e267e41000, b=0x55e267d69a00, lock_type_held=SIX_LOCK_intent, flags=<optimized out>) at libbcachefs/btree_io.c:2677
#2  0x000055e232b19752 in btree_split (as=as@entry=0x55e267e45800, trans=trans@entry=0x55e267e41000, path=path@entry=4, b=b@entry=0x55e267d6e900, keys=keys@entry=0x0) at libbcachefs/btree_update_interior.c:1798
#3  0x000055e232b1b799 in bch2_btree_split_leaf (trans=trans@entry=0x55e267e41000, path=4, flags=flags@entry=48) at libbcachefs/btree_update_interior.c:1959
#4  0x000055e232b0ed78 in bch2_trans_commit_error (trace_ip=<optimized out>, trans=<optimized out>, flags=<optimized out>, i=<optimized out>, ret=<optimized out>) at libbcachefs/btree_trans_commit.c:918
#5  __bch2_trans_commit (trans=trans@entry=0x55e267e41000, flags=flags@entry=(BCH_TRANS_COMMIT_no_enospc | BCH_TRANS_COMMIT_no_check_rw)) at libbcachefs/btree_trans_commit.c:1143
#6  0x000055e232b8208d in bch2_trans_commit (trans=0x55e267e41000, disk_res=0x7ffe75f78cd8, journal_seq=0x0, flags=48) at libbcachefs/btree_update.h:287
#7  bch2_extent_update (trans=trans@entry=0x55e267e41000, inum=..., iter=iter@entry=0x7ffe75f78910, k=0x7ffe75f78a38, disk_res=disk_res@entry=0x7ffe75f78cd8, new_i_size=7, i_sectors_delta_total=0x7ffe75f78d10,
    check_enospc=false) at libbcachefs/io_write.c:356
#8  0x000055e232b82435 in bch2_write_index_default (op=0x7ffe75f78be0) at libbcachefs/io_write.c:402
#9  0x000055e232b82c25 in __bch2_write_index (op=op@entry=0x7ffe75f78be0) at libbcachefs/io_write.c:599
#10 0x000055e232b86eca in bch2_write_data_inline (op=0x7ffe75f78be0, data_len=<optimized out>) at libbcachefs/io_write.c:1645
#11 bch2_write (ws=0x7ffe75f78be0) at libbcachefs/io_write.c:1715
#12 0x000055e232aa075b in closure_queue (cl=0x7ffe75f78be0) at include/linux/closure.h:270
#13 closure_call (cl=0x7ffe75f78be0, fn=<optimized out>, wq=0x0, parent=0x0) at include/linux/closure.h:432
#14 write_aligned (c=c@entry=0x7f04a1b8a000, inum=..., io_opts=..., buf=buf@entry=0x55e267e40000, aligned_size=aligned_size@entry=512, aligned_offset=aligned_offset@entry=0, new_i_size=<optimized out>,
    written_out=<optimized out>) at c_src/cmd_fusemount.c:604
#15 0x000055e232aa20d7 in bcachefs_fuse_write (req=0x55e267e35f00, ino=<optimized out>, buf=0x7f049feb6000 "skfjsl\n", size=7, offset=<optimized out>, fi=<optimized out>) at c_src/cmd_fusemount.c:670
#16 0x00007f04a1c1064e in do_write (req=<optimized out>, nodeid=<optimized out>, inarg=<optimized out>) at ../lib/fuse_lowlevel.c:1457
#17 0x00007f04a1c1f19b in fuse_session_process_buf_internal (se=0x55e267f65770, buf=0x7ffe75f79360, ch=<optimized out>) at ../lib/fuse_lowlevel.c:2909
#18 0x00007f04a1c1681d in fuse_session_loop (se=se@entry=0x55e267f65770) at ../lib/fuse_loop.c:34
#19 0x000055e232aa3108 in cmd_fusemount (argc=<optimized out>, argv=<optimized out>) at c_src/cmd_fusemount.c:1273
#20 0x000055e232a58547 in bcachefs::handle_c_command (argv=..., symlink_cmd=...) at src/bcachefs.rs:74
#21 0x000055e232a58bcd in bcachefs::main () at src/bcachefs.rs:122


#0  __bch2_btree_node_write (c=c@entry=0x7f04a1b8a000, b=b@entry=0x55e267d69a00, flags=flags@entry=8) at libbcachefs/btree_io.c:2313
#1  0x000055e232af1263 in bch2_btree_node_write_trans (trans=0x7f047c001000, b=0x55e267d69a00, lock_type_held=SIX_LOCK_read, flags=8) at libbcachefs/btree_io.c:2677
#2  0x000055e232b168e8 in btree_node_write_if_need (trans=0x7f047c001000, b=0x55e267d69a00, lock_held=SIX_LOCK_read) at libbcachefs/btree_io.h:167
#3  btree_update_nodes_written (as=0x55e267e45800) at libbcachefs/btree_update_interior.c:883
#4  btree_interior_update_work (work=0x7f04a1b8d7e8) at libbcachefs/btree_update_interior.c:910
#5  0x000055e232bfeb3b in worker_thread (arg=0x55e267d77500) at linux/workqueue.c:262
#6  0x000055e232bf7bb3 in kthread_start_fn (data=0x55e267d77550) at linux/kthread.c:25
#7  0x00007f04a18969cb in start_thread (arg=<optimized out>) at pthread_create.c:448
#8  0x00007f04a191aa0c in __GI___clone3 () at ../sysdeps/unix/sysv/linux/x86_64/clone3.S:78
```
