# test


## centos

### 服务端
```shell
yum install targetcli

[root@localhost ~]# targetcli
targetcli   targetclid
[root@localhost ~]# targetcli
Warning: Could not load preferences file /root/.targetcli/prefs.bin.
targetcli shell version 2.1.53
Copyright 2011-2013 by Datera, Inc and others.
For help on commands, type 'help'.

/> /iscsi
/iscsi> ls
o- iscsi .............................................................................................................. [Targets: 0]
/iscsi> create wwn=iqn.2021-03.com.iscsi:server
Created target iqn.2021-03.com.iscsi:server.
Created TPG 1.
Global pref auto_add_default_portal=true
Created default portal listening on all IPs (0.0.0.0), port 3260.
/iscsi> /iscsi/iqn.2021-03.com.iscsi:server/tpg1/acls> create wwn=iqn.2021-03.com.iscsi:client
/iscsi> /iscsi/iqn.2021-03.com.iscsi:server/tpg1/acls
/iscsi/iqn.20...ver/tpg1/acls> create wwn=iqn.2021-03.com.iscsi:client
Created Node ACL for iqn.2021-03.com.iscsi:client
/iscsi/iqn.20...ver/tpg1/acls> /iscsi/iqn.2021-03.com.iscsi:server/tpg1/acls/iqn.2021-03.com.iscsi:client
/iscsi/iqn.20....iscsi:client> set auth userid=username password=password
Parameter password is now 'password'.
Parameter userid is now 'username'.
/iscsi/iqn.20....iscsi:client> /backstores/block
/backstores/block> create name=centos-home dev=/dev/mapper/centos-home
Cannot configure StorageObject because device /dev/mapper/centos-home is already in use
/backstores/block> create name=centos-home dev=/dev/mapper/centos-home
Created block storage object centos-home using /dev/mapper/centos-home.
/backstores/block> /iscsi/iqn.2021-03.com.iscsi:server/tpg1/luns
/iscsi/iqn.20...ver/tpg1/luns> create /backstores/block/centos-home
Created LUN 0.
Created LUN 0->0 mapping in node ACL iqn.2021-03.com.iscsi:client
/iscsi/iqn.20...ver/tpg1/luns> /iscsi/iqn.2021-03.com.iscsi:server/tpg1/portals/
/iscsi/iqn.20.../tpg1/portals> ls
o- portals ............................................................................................................ [Portals: 1]
  o- 0.0.0.0:3260 ............................................................................................................. [OK]
/iscsi/iqn.20.../tpg1/portals> /
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


```

### 客户端
```shell
yum install iscsi-initiator-utils -y

cat /etc/iscsi/initiatorname.iscsi
iqn.2021-03.com.iscsi:client

vi /etc/iscsi/iscsid.conf
# To set a CHAP username and password for initiator
# authentication by the target(s), uncomment the following lines:
node.session.auth.username = username
node.session.auth.password = password

# To set a CHAP username and password for target(s)
# authentication by the initiator, uncomment the following lines:
node.session.auth.username_in = username_in
node.session.auth.password_in = password_in

[root@localhost appDxc_mountpoint]# systemctl start iscsid
[root@localhost appDxc_mountpoint]# systemctl status iscsid
● iscsid.service - Open-iSCSI
   Loaded: loaded (/usr/lib/systemd/system/iscsid.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2025-10-09 23:48:04 EDT; 19min ago
     Docs: man:iscsid(8)
           man:iscsiuio(8)
           man:iscsiadm(8)
 Main PID: 4882 (iscsid)
   Status: "Ready to process requests"
   CGroup: /system.slice/iscsid.service
           └─4882 /sbin/iscsid -f

Oct 09 23:48:04 localhost.localdomain systemd[1]: Starting Open-iSCSI...
Oct 09 23:48:04 localhost.localdomain systemd[1]: Started Open-iSCSI.


[root@localhost ~]# iscsiadm -m discovery -t sendtargets -p 10.6.66.165
10.6.66.165:3260,1 iqn.2021-03.com.iscsi:server


[root@localhost ~]# iscsiadm -m node -T iqn.2021-03.com.iscsi:server -p 10.6.66.165 -l
Logging in to [iface: default, target: iqn.2021-03.com.iscsi:server, portal: 10.6.66.165,3260] (multiple)
Login to [iface: default, target: iqn.2021-03.com.iscsi:server, portal: 10.6.66.165,3260] successful.
```




