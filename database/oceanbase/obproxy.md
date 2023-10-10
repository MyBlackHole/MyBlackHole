# obproxy

```shell
/opt/taobao/install/obproxy-3.3.1/bin/obproxy -p 2883 -l 2884 -n default -o enable_cached_server=false,enable_get_rslist_remote=true,monitor_stat_dump_interval=1s,enable_qos=true,enable_standby=false,query_digest_time_threshold=2ms,monitor_cost_ms_unit=true,enable_strict_kernel_release=false,obproxy_config_server_url=http://api.test.ocp.oceanbase.alibaba.net/services?Action=GetObRootServiceInfoUrlTemplate&User_ID=alibaba-inc&UID=ocpmaster,enable_proxy_scramble=true,work_thread_num=8,proxy_mem_limited=2G,log_dir_size_threshold=10G,proxy_idc_name=,bt_server_addr=,bt_antvip_server_addr=,bt_server_antvip=,bt_local_work_mode=true,dataplane_host=pilot-dds-.cloudmesh.:15010,sidecar_node_id=sidecar~192.168.80.140~wdg-oceanbase-80-140.~.svc.cluster.local,domain_name=.alipay.net,env_tenant_name=ALIPAY,workspace_name=dev,server_zone=,pod_name=wdg-oceanbase-80-140,pod_namespace=,instance_ip=,enable_sharding=true
```

-n,--appname APPNAME application name 对应集群名称。配置obproxy需要使用到。
observer [OPTIONS]
-h,--help print this help
-V,--version print the information of version
-z,--zone ZONE zone
-p,--mysql_port PORT mysql port
-P,--rpc_port PORT rpc port
-N,--nodaemon don't run in daemon
-n,--appname APPNAME application name
-c,--cluster_id ID cluster id
-d,--data_dir DIR OceanBase data directory
-i,--devname DEV net dev interface
-o,--optstr OPTSTR extra options string
-r,--rs_list RS_LIST root service list
-l,--log_level LOG_LEVEL server log level
-6,--ipv6 USE_IPV6 server use ipv6 address
-m,--mode MODE server mode
-f,--scn flashback_scn

