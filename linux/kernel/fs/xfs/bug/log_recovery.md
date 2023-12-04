<!-- 日志损坏 -->
<!-- growfs + power-off 测试过程中 -->
<!-- 在日志记录中，修改事务的扩展区块位于 growfs 交易之前，这导致验证日志重播期间失败。 -->
[细节](https://gitee.com/openeuler/kernel/commit/dba19fb8c8265bd66b689403c62ba87e74382425)


[97871.215978] XFS (sdb): xfs_buf_find: daddr 0x400003 out of range, EOFS 0x400000
[97871.216131] ------------[ cut here ]------------
[97871.216177] WARNING: CPU: 1 PID: 52131 at fs/xfs/xfs_buf.c:604 xfs_buf_find.isra.28+0x5ab/0x5c0 [xfs]
[97871.216180] Modules linked in: tcp_diag udp_diag raw_diag inet_diag unix_diag af_packet_diag netlink_diag iscsi_tcp libiscsi_tcp libiscsi rfkill scsi_transport_iscsi binfmt_misc nfit libnvdimm crct10dif_pclmul crc32_pclmul ghash_clmulni_intel joydev vmw_balloon intel_rapl_perf sg psmouse pcspkr vmw_vmci i2c_piix4 ip_tables xfs libcrc32c sr_mod cdrom sd_mod ata_generic vmwgfx drm_kms_helper syscopyarea sysfillrect crc32c_intel sysimgblt fb_sys_fops ttm serio_raw drm ahci vmxnet3 libahci ata_piix libata vmw_pvscsi dm_mirror dm_region_hash dm_log dm_mod
[97871.216191] CPU: 1 PID: 52131 Comm: mount Kdump: loaded Tainted: G        W         4.19.90-24.4.v2101.ky10.x86_64 #1
[97871.216192] Hardware name: VMware, Inc. VMware Virtual Platform/440BX Desktop Reference Platform, BIOS 6.00 11/12/2020
[97871.216217] RIP: 0010:xfs_buf_find.isra.28+0x5ab/0x5c0 [xfs]
[97871.216219] Code: 00 00 01 e9 73 fe ff ff 48 89 c1 48 c7 c2 10 66 6d c0 48 c7 c6 c8 d9 6d c0 e8 c1 78 01 00 48 c7 c7 f8 d9 6d c0 e8 23 7a 28 fd <0f> 0b 41 ba 8b ff ff ff e9 44 fe ff ff e8 23 04 22 fd 0f 1f 00 0f
[97871.216220] RSP: 0018:ffffb65b432078a8 EFLAGS: 00010286
[97871.216221] RAX: 0000000000000024 RBX: ffff9937e0307400 RCX: 0000000000000006
[97871.216222] RDX: 0000000000000000 RSI: 0000000000000096 RDI: ffff99387fc96890
[97871.216222] RBP: 0000000000000000 R08: 0000000000001e69 R09: 0000000000000004
[97871.216223] R10: 000000000000123c R11: 0000000000000001 R12: ffffb65b432079c8
[97871.216224] R13: ffffb65b43207930 R14: ffff99382fbe2e18 R15: ffff99382fbe2e18
[97871.216225] FS:  00007f9305eab0c0(0000) GS:ffff99387fc80000(0000) knlGS:0000000000000000
[97871.216226] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[97871.216226] CR2: 00007ffc2d57ff88 CR3: 00000002f0afa002 CR4: 00000000007606e0
[97871.216232] PKRU: 55555554
[97871.216232] Call Trace:
[97871.216255]  xfs_buf_get_map+0x40/0x2a0 [xfs]
[97871.216275]  xfs_buf_read_map+0x28/0x170 [xfs]
[97871.216298]  xlog_recover_buffer_pass2+0x95/0x510 [xfs]
[97871.216321]  xlog_recover_items_pass2+0x3c/0x60 [xfs]
[97871.216347]  xlog_recover_commit_trans+0x294/0x2b0 [xfs]
[97871.216374]  xlog_recovery_process_trans+0x8e/0xc0 [xfs]
[97871.216399]  xlog_recover_process_data+0xa3/0x120 [xfs]
[97871.216419]  xlog_do_recovery_pass+0x3d6/0x5d0 [xfs]
[97871.216422]  ? __irq_work_queue_local+0x20/0x60
[97871.216423]  ? vprintk_emit+0x11f/0x2a0
[97871.216440]  xlog_do_log_recovery+0x7f/0xb0 [xfs]
[97871.216458]  xlog_do_recover+0x2c/0x180 [xfs]
[97871.216475]  xlog_recover+0xc0/0x170 [xfs]
[97871.216497]  xfs_log_mount+0x17a/0x2b0 [xfs]
[97871.216519]  xfs_mountfs+0x528/0x860 [xfs]
[97871.216541]  ? xfs_mru_cache_create+0x12e/0x190 [xfs]
[97871.216562]  xfs_fs_fill_super+0x469/0x610 [xfs]
[97871.216584]  ? xfs_test_remount_options+0x60/0x60 [xfs]
[97871.216586]  mount_bdev+0x173/0x1b0
[97871.216588]  mount_fs+0x35/0x160
[97871.216590]  vfs_kern_mount.part.28+0x54/0x120
[97871.216592]  do_mount+0x5c2/0xc60
[97871.216593]  ? _copy_from_user+0x37/0x60
[97871.216595]  ? kmem_cache_alloc_trace+0x2f/0x1a0
[97871.216596]  ksys_mount+0x80/0xd0
[97871.216598]  __x64_sys_mount+0x21/0x30
[97871.216600]  do_syscall_64+0x5b/0x1d0
[97871.216602]  entry_SYSCALL_64_after_hwframe+0x44/0xa9
[97871.216604] RIP: 0033:0x7f93060671fa
[97871.216605] Code: 48 8b 0d a9 7c 0b 00 f7 d8 64 89 01 48 83 c8 ff c3 66 2e 0f 1f 84 00 00 00 00 00 0f 1f 44 00 00 49 89 ca b8 a5 00 00 00 0f 05 <48> 3d 01 f0 ff ff 73 01 c3 48 8b 0d 76 7c 0b 00 f7 d8 64 89 01 48
[97871.216605] RSP: 002b:00007ffc2d581128 EFLAGS: 00000246 ORIG_RAX: 00000000000000a5
[97871.216606] RAX: ffffffffffffffda RBX: 000055564e89fa50 RCX: 00007f93060671fa
[97871.216607] RDX: 000055564e89fc80 RSI: 000055564e89fd00 RDI: 000055564e89fce0
[97871.216607] RBP: 0000000000000000 R08: 000055564e89fca0 R09: 000055564e89e010
[97871.216607] R10: 0000000000000000 R11: 0000000000000246 R12: 000055564e89fce0
[97871.216608] R13: 000055564e89fc80 R14: 0000000000000001 R15: 00007f930620a224
[97871.216609] ---[ end trace 90f7f7830dcd4193 ]---
[97871.216910] XFS (sdb): log mount/recovery failed: error -12
[97871.220126] XFS (sdb): log mount failed


```c
diff --git a/fs/xfs/libxfs/xfs_log_recover.h b/fs/xfs/libxfs/xfs_log_recover.h
index 32a8ab3da18371a1889489cd5f9a58c1d054f0fa..27732685b8aebed0ae21157c3effd8e8d20ef70f 100644
--- a/fs/xfs/libxfs/xfs_log_recover.h
+++ b/fs/xfs/libxfs/xfs_log_recover.h
@@ -18,6 +18,7 @@ enum xlog_recover_reorder {
 	XLOG_REORDER_ITEM_LIST,
 	XLOG_REORDER_INODE_BUFFER_LIST,
 	XLOG_REORDER_CANCEL_LIST,
+	XLOG_REORDER_SB_BUFFER_LIST,
 };
 
 struct xlog_recover_item_ops {
diff --git a/fs/xfs/xfs_buf_item_recover.c b/fs/xfs/xfs_buf_item_recover.c
index 368937745f809b7ec9e15cd8fc3fca3c9f5e7a65..9cb79a494d06bf002745b35c32bb31feba67ef49 100644
--- a/fs/xfs/xfs_buf_item_recover.c
+++ b/fs/xfs/xfs_buf_item_recover.c
@@ -162,6 +162,8 @@ xlog_recover_buf_reorder(
 		return XLOG_REORDER_CANCEL_LIST;
 	if (buf_f->blf_flags & XFS_BLF_INODE_BUF)
 		return XLOG_REORDER_INODE_BUFFER_LIST;
+	if (buf_f->blf_blkno == XFS_SB_DADDR)
+		return XLOG_REORDER_SB_BUFFER_LIST;
 	return XLOG_REORDER_BUFFER_LIST;
 }
 
diff --git a/fs/xfs/xfs_log_recover.c b/fs/xfs/xfs_log_recover.c
index 7d2a788b3e7ebcbbd95f720535897b7e0126e697..8235a1e05ee848b36fca22140239cec3eef3b4d2 100644
--- a/fs/xfs/xfs_log_recover.c
+++ b/fs/xfs/xfs_log_recover.c
@@ -1879,6 +1879,9 @@ xlog_recover_reorder_trans(
 		case XLOG_REORDER_BUFFER_LIST:
 			list_move_tail(&item->ri_list, &buffer_list);
 			break;
+		case XLOG_REORDER_SB_BUFFER_LIST:
+			list_move(&item->ri_list, &buffer_list);
+			break;
 		case XLOG_REORDER_CANCEL_LIST:
 			trace_xfs_log_recover_item_reorder_head(log,
 					trans, item, pass);
@@ -1942,6 +1945,25 @@ xlog_recover_items_pass2(
 	return error;
 }
 
+#define XLOG_RECOVER_COMMIT_QUEUE_MAX 100
+static inline bool
+xlog_recover_should_pass2(
+	struct xlog_recover_item	*item,
+	int				items_queued)
+{
+	struct xfs_buf_log_format	*buf_f;
+
+	if (items_queued >= XLOG_RECOVER_COMMIT_QUEUE_MAX)
+		return true;
+	if (ITEM_TYPE(item) == XFS_LI_BUF) {
+		buf_f = item->ri_buf[0].i_addr;
+		if (buf_f->blf_blkno == XFS_SB_DADDR)
+			return true;
+	}
+
+	return false;
+}
+
 /*
  * Perform the transaction.
  *
@@ -1962,8 +1984,6 @@ xlog_recover_commit_trans(
 	LIST_HEAD			(ra_list);
 	LIST_HEAD			(done_list);
 
-	#define XLOG_RECOVER_COMMIT_QUEUE_MAX 100
-
 	hlist_del_init(&trans->r_list);
 
 	error = xlog_recover_reorder_trans(log, trans, pass);
@@ -1983,7 +2003,7 @@ xlog_recover_commit_trans(
 				item->ri_ops->ra_pass2(log, item);
 			list_move_tail(&item->ri_list, &ra_list);
 			items_queued++;
-			if (items_queued >= XLOG_RECOVER_COMMIT_QUEUE_MAX) {
+			if (xlog_recover_should_pass2(item, items_queued)) {
 				error = xlog_recover_items_pass2(log, trans,
 						buffer_list, &ra_list);
 				list_splice_tail_init(&ra_list, &done_list);
```
