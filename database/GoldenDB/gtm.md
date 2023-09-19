# gtm

- 查询 gtm 信息
```shell
select a.gtm_id, b.db_cluster_id, a.gtm_backup, a.gtm_ip as node_ipaddr, c.appuser_name as node_os_username from mds.gtm_info as a join mds.gtm_db_bind_info as b join goldendb_omm.gdb_cityinstall_gtm as c on a.gtm_cluster_id=b.gtm_cluster_id and a.gtm_ip=c.gtm_ip and a.gtm_port=c.listen_port and a.gtm_srcport=c.source_port where a.gtm_status=2;
```
