
[root@node3 class]# ls -alh|grep iscsi
drwxr-xr-x  2 root root 0 Dec 25 16:34 iscsi_connection
drwxr-xr-x  2 root root 0 Dec 25 16:34 iscsi_endpoint
drwxr-xr-x  2 root root 0 Dec 25 16:34 iscsi_host
drwxr-xr-x  2 root root 0 Dec 25 16:34 iscsi_iface
drwxr-xr-x  2 root root 0 Dec 25 16:34 iscsi_session
drwxr-xr-x  2 root root 0 Dec 25 16:34 iscsi_transport
[root@node3 class]# ls /sys/class/ | grep ^scsi
drwxr-xr-x  2 root root 0 Dec 25 16:35 scsi_device
drwxr-xr-x  2 root root 0 Dec 25 16:35 scsi_disk
drwxr-xr-x  2 root root 0 Dec 25 16:35 scsi_generic
drwxr-xr-x  2 root root 0 Dec 25 16:35 scsi_host
[root@node3 class]# pwd
/sys/class



[root@node3 iscsi]# pwd
/var/lib/iscsi
[root@node3 iscsi]# tree
.
├── ifaces
├── isns
├── nodes
│   ├── iqn.2019-09.com.example:disk
│   │   └── 192.168.90.45,3260,1
│   │       └── default
│   └── iqn.2023-12.activeio.com:22192.168.78.214-1703685960.840158server
│       └── 192.168.78.213,3260,1
│           └── default
├── send_targets
│   ├── 192.168.100.179,3260
│   │   └── st_config
│   ├── 192.168.78.213,3260
│   │   ├── iqn.2023-12.activeio.com:22192.168.78.214-1703685960.840158server,192.168.78.213,3260,1,default -> /var/lib/iscsi/nodes/iqn.2023-12.activeio.com:22192.168.78.214-1703685960.840158server/192.168.78.213,3260,1
│   │   └── st_config
│   └── 192.168.90.45,3260
│       ├── iqn.2019-09.com.example:disk,192.168.90.45,3260,1,default -> /var/lib/iscsi/nodes/iqn.2019-09.com.example:disk/192.168.90.45,3260,1
│       └── st_config
├── slp
└── static

15 directories, 5 files


[root@node3 192.168.78.213,3260,1]# pwd
/var/lib/iscsi/nodes/iqn.2023-12.activeio.com:22192.168.78.214-1703685960.840158server/192.168.78.213,3260,1
[root@node3 192.168.78.213,3260,1]# cat default
# BEGIN RECORD 6.2.0.874-22
node.name = iqn.2023-12.activeio.com:22192.168.78.214-1703685960.840158server
node.tpgt = 1
node.startup = automatic
node.leading_login = No
iface.iscsi_ifacename = default
iface.transport_name = tcp
iface.vlan_id = 0
iface.vlan_priority = 0
iface.iface_num = 0
iface.mtu = 0
iface.port = 0
iface.tos = 0
iface.ttl = 0
iface.tcp_wsf = 0
iface.tcp_timer_scale = 0
iface.def_task_mgmt_timeout = 0
iface.erl = 0
iface.max_receive_data_len = 0
iface.first_burst_len = 0
iface.max_outstanding_r2t = 0
iface.max_burst_len = 0
node.discovery_address = 192.168.78.213
node.discovery_port = 3260
node.discovery_type = send_targets
node.session.initial_cmdsn = 0
node.session.initial_login_retry_max = 8
node.session.xmit_thread_priority = -20
node.session.cmds_max = 128
node.session.queue_depth = 32
node.session.nr_sessions = 1
node.session.auth.authmethod = CHAP
node.session.auth.username = activeio
node.session.auth.password = activeio
node.session.auth.chap_algs = MD5
node.session.timeo.replacement_timeout = 120
node.session.err_timeo.abort_timeout = 15
node.session.err_timeo.lu_reset_timeout = 30
node.session.err_timeo.tgt_reset_timeout = 30
node.session.err_timeo.host_reset_timeout = 60
node.session.iscsi.FastAbort = Yes
node.session.iscsi.InitialR2T = No
node.session.iscsi.ImmediateData = Yes
node.session.iscsi.FirstBurstLength = 262144
node.session.iscsi.MaxBurstLength = 16776192
node.session.iscsi.DefaultTime2Retain = 0
node.session.iscsi.DefaultTime2Wait = 2
node.session.iscsi.MaxConnections = 1
node.session.iscsi.MaxOutstandingR2T = 1
node.session.iscsi.ERL = 0
node.session.scan = auto
node.conn[0].address = 192.168.78.213
node.conn[0].port = 3260
node.conn[0].startup = manual
node.conn[0].tcp.window_size = 524288
node.conn[0].tcp.type_of_service = 0
node.conn[0].timeo.logout_timeout = 15
node.conn[0].timeo.login_timeout = 15
node.conn[0].timeo.auth_timeout = 45
node.conn[0].timeo.noop_out_interval = 5
node.conn[0].timeo.noop_out_timeout = 5
node.conn[0].iscsi.MaxXmitDataSegmentLength = 0
node.conn[0].iscsi.MaxRecvDataSegmentLength = 262144
node.conn[0].iscsi.HeaderDigest = None
node.conn[0].iscsi.IFMarker = No
node.conn[0].iscsi.OFMarker = No
# END RECORD
