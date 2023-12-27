# iscsiadm

## discovery
- 发现 target
```shell
sudo iscsiadm -m discovery -t sendtargets -p 192.168.100.179
192.168.100.179:3260,1 iqn.2023-05.black.com:server
192.168.100.179:3260,1 iqn.2003-01.org.linux-iscsi.black.x8664:sn.dbf8950fa890
```

```shell
ls -l /dev/disk/by-path
```

## node

- 所有记录
```shell
iscsiadm -m node
```

- 开启认证
```shell
iscsiadm -m node -T iqn.2019-09.com.example:disk -o update --name node.session.auth.authmethod --value=CHAP

```

- 添加用户
```shell
iscsiadm -m node -T iqn.2019-09.com.example:disk --op  update --name node.session.auth.username --value=wangyujie
```

- 添加密码
```shell
iscsiadm -m node -T iqn.2019-09.com.example:disk --op update --name node.session.auth.password --value=wangyujie
```

- 开启 debug login
```shell
iscsiadm -m node -T iqn.2019-09.com.example:disk -p 192.168.90.45 -l
```

- 退出登陆
```shell
iscsiadm -m node –T iqn.2023-05.black.com:server -p 192.168.1.1:3260 –u
iscsiadm -m node –T iqn.2023-09.activeio.com:22192.168.79.23-1694665184.882196server -p 192.168.78.213:3260 –u

sudo iscsiadm -m node -T iqn.2023-09.activeio.com:22192.168.79.23-1694665184.882196server -u

```

- 删除 target
```shell
iscsiadm -m node -o delete -T iqn.2023-05.black.com:server -p 192.168.1.1:3260
iscsiadm -m node -o delete -T iqn.2023-09.activeio.com:22192.168.79.23-1694665184.882196server
```

## session
- 刷新资源
```shell
sudo iscsiadm -m session –R
```

- 
```shell
sudo iscsiadm -m session –P 2
Target: iqn.2023-05.black.com:server (non-flash)
        Current Portal: 192.168.100.179:3260,1
        Persistent Portal: 192.168.100.179:3260,1
                **********
                Interface:
                **********
                Iface Name: default
                Iface Transport: tcp
                Iface Initiatorname: iqn.2023-05.black.com:client
                Iface IPaddress: 192.168.100.179
                Iface HWaddress: default
                Iface Netdev: default
                SID: 6
                iSCSI Connection State: LOGGED IN
                iSCSI Session State: LOGGED_IN
                Internal iscsid Session State: NO CHANGE
                *********
                Timeouts:
                *********
                Recovery Timeout: 120
                Target Reset Timeout: 30
                LUN Reset Timeout: 30
                Abort Timeout: 15
                *****
                CHAP:
                *****
                username: black
                password: ********
                username_in: <empty>
                password_in: ********
                ************************
                Negotiated iSCSI params:
                ************************
                HeaderDigest: None
                DataDigest: None
                MaxRecvDataSegmentLength: 262144
                MaxXmitDataSegmentLength: 262144
                FirstBurstLength: 65536
                MaxBurstLength: 262144
                ImmediateData: Yes
                InitialR2T: Yes
                MaxOutstandingR2T: 1
```
