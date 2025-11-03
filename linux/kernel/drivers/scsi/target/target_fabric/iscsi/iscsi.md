# iscsi

```shell
/sys/kernel/config/target/iscsi

/> ls
o- / ......................................................................................................................... [...]
  o- backstores .............................................................................................................. [...]
  | o- block .................................................................................................. [Storage Objects: 1]
  | | o- centos-home ....................................................... [/dev/mapper/centos-home (2.0TiB) write-thru activated]
  | |   o- alua ................................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ....................................................................... [ALUA state: Active/optimized]
  | o- fileio ................................................................................................. [Storage Objects: 0]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2021-03.com.iscsi:server ...................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 1]
  |     | o- iqn.2021-03.com.iscsi:client ......................................................................... [Mapped LUNs: 1]
  |     |   o- mapped_lun0 ........................................................................... [lun0 block/centos-home (rw)]
  |     o- luns .......................................................................................................... [LUNs: 1]
  |     | o- lun0 ................................................. [block/centos-home (/dev/mapper/centos-home) (default_tg_pt_gp)]
  |     o- portals .................................................................................................... [Portals: 1]
  |       o- 0.0.0.0:3260 ..................................................................................................... [OK]
  o- loopback ......................................................................................................... [Targets: 0]
/> exit

cat /sys/kernel/config/target/core/iblock_0/centos-home/attrib/emulate_write_cache
0
```

```c
// target 构造操作集配置
const struct target_core_fabric_ops iscsi_ops = {
	.module				= THIS_MODULE,
	.fabric_alias			= "iscsi",
	.fabric_name			= "iSCSI",
	.node_acl_size			= sizeof(struct iscsi_node_acl),
	.tpg_get_wwn			= lio_tpg_get_endpoint_wwn,
	.tpg_get_tag			= lio_tpg_get_tag,
	.tpg_get_default_depth		= lio_tpg_get_default_depth,
	.tpg_check_demo_mode		= lio_tpg_check_demo_mode,
	.tpg_check_demo_mode_cache	= lio_tpg_check_demo_mode_cache,
	.tpg_check_demo_mode_write_protect =
			lio_tpg_check_demo_mode_write_protect,
	.tpg_check_prod_mode_write_protect =
			lio_tpg_check_prod_mode_write_protect,
	.tpg_check_prot_fabric_only	= &lio_tpg_check_prot_fabric_only,
	.tpg_get_inst_index		= lio_tpg_get_inst_index,
	.check_stop_free		= lio_check_stop_free,
	.release_cmd			= lio_release_cmd,
	.close_session			= lio_tpg_close_session,
	.sess_get_index			= lio_sess_get_index,
	.sess_get_initiator_sid		= lio_sess_get_initiator_sid,
	.write_pending			= lio_write_pending,
	.set_default_node_attributes	= lio_set_default_node_attributes,
	.get_cmd_state			= iscsi_get_cmd_state,
	.queue_data_in			= lio_queue_data_in,
	.queue_status			= lio_queue_status,
	.queue_tm_rsp			= lio_queue_tm_rsp,
	.aborted_task			= lio_aborted_task,
	.fabric_make_wwn		= lio_target_call_coreaddtiqn,
	.fabric_drop_wwn		= lio_target_call_coredeltiqn,
	.add_wwn_groups			= lio_target_add_wwn_groups,
	.fabric_make_tpg		= lio_target_tiqn_addtpg,
	.fabric_enable_tpg		= lio_target_tiqn_enabletpg,
	.fabric_drop_tpg		= lio_target_tiqn_deltpg,
    // 构造
	.fabric_make_np			= lio_target_call_addnptotpg,
	.fabric_drop_np			= lio_target_call_delnpfromtpg,
	.fabric_init_nodeacl		= lio_target_init_nodeacl,

    // // configfs 属性(对应到 /sys/kernel/config/target/iscsi 下目录与文件)
    // configfs 属性(对应到 /sys/kernel/config/target/iscsi/discovery_auth/ 下目录与文件)
	.tfc_discovery_attrs		= lio_target_discovery_auth_attrs,
	.tfc_wwn_attrs			= lio_target_wwn_attrs,
	.tfc_tpg_base_attrs		= lio_target_tpg_attrs,
	.tfc_tpg_attrib_attrs		= lio_target_tpg_attrib_attrs,
	.tfc_tpg_auth_attrs		= lio_target_tpg_auth_attrs,
	.tfc_tpg_param_attrs		= lio_target_tpg_param_attrs,
	.tfc_tpg_np_base_attrs		= lio_target_portal_attrs,
	.tfc_tpg_nacl_base_attrs	= lio_target_initiator_attrs,
	.tfc_tpg_nacl_attrib_attrs	= lio_target_nacl_attrib_attrs,
	.tfc_tpg_nacl_auth_attrs	= lio_target_nacl_auth_attrs,
	.tfc_tpg_nacl_param_attrs	= lio_target_nacl_param_attrs,

	.write_pending_must_be_called	= 1,

	.default_submit_type		= TARGET_DIRECT_SUBMIT,
	.direct_submit_supp		= 1,
};
```
