# proxy

```shell
select a.proxy_name, a.proxy_id, a.proxy_ip as node_ipaddr, b.cluster_id, c.appuser_name as node_os_username from mds.proxy_info as a join mds.cluster_proxy_bind_info as b join goldendb_omm.gdb_cityinstall_dbproxy as c on a.proxy_id=b.proxy_id and a.proxy_ip=c.host_ip and a.proxy_port=c.listen_port where a.proxy_status=1 and b.status=0;
```
