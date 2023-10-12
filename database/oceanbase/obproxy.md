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


```shell
/opt/taobao/install/obproxy-3.3.1/bin/obproxy -p 2883 -n ob110145146147_proxy
```

- 接入 obproxy
```shell
<!-- 给集群设置密码 -->
grant select on oceanbase.* to proxyro identified by 'wwDD99__ob';


<!-- 启动 -->
/opt/taobao/install/obproxy-3.3.1/bin/obproxy -p 2883 -r '192.168.80.140:2881;192.168.80.141:2881;192.168.80.142:2881' -c wdg80140141142 -n wdg80140141142
/opt/taobao/install/obproxy-3.3.1/bin/obproxy -p 2883 -r '192.168.80.140:2881;192.168.80.141:2881;192.168.80.142:2881' -o enable_strict_kernel_release=false,enable_cluster_checkout=false -c obdemo

/opt/taobao/install/obproxy-3.3.1/bin/obproxy -p 2883 -r 192.168.80.140:2881;192.168.80.141:2881;192.168.80.142:2881 -c wdg80140141142 -n wdg80140141142
listen port: 2883
rs list: 192.168.80.140:2881;192.168.80.141:2881;192.168.80.142:2881
cluster_name: wdg80140141142
appname: wdg80140141142

<!-- proxysys 默认密码为空 -->
mysql -h127.0.0.1 -P2883 -uroot@proxysys

<!-- 设置 obproxy 自己的连接密码  -->
alter proxyconfig set obproxy_sys_password = 'wwDD99__ob';


(1). 任一节点下，登陆OB集群sys租户，创建obproxy的内部proxyro用户，并设置密码
mysql -h127.1 -uroot@sys -P2881 -p -Doceanbase
mysql> create user if not exists proxyro identified by 'admin123123';
mysql> grant select on *.* to proxyro;

(2). 以obproxy管理员账号root@proxysys登录OBProxy，设置proxyro@sys的密码为上一步OB集群创建的proxyro用户的密码
mysql -h127.0.0.1 -P2883 -uroot@proxysys -p'admin12345'
mysql> alter proxyconfig set observer_sys_password = 'admin123123';

(3). 配置obproxy参数
以obproxy管理员账号root@proxysys登录OBProxy
mysql -h127.0.0.1 -P2883 -uroot@proxysys -p'admin12345'


(4). 设置密码和配置完obproxy参数后，建议重启obproxy进程，以admin用户执行：
su - admin
pkill obproxy
cd /opt/taobao/install/obproxy && ./bin/obproxy

# 检查obproxy进程是否启动成功
ps -ef | grep obproxy
```

