- 19 encountered non-retryable iSCSI login failure
```shell
Logging in to [iface: default, target: iqn.2023-05.black.com:server, portal: 192.168.100.179,3260]
iscsiadm: Could not login to [iface: default, target: iqn.2023-05.black.com:server, portal: 192.168.100.179,3260].
iscsiadm: initiator reported error (19 - encountered non-retryable iSCSI login failure)
iscsiadm: Could not log into all portals


双方验证方式信息不一致, 修改过后需要重新发现
```

- 24 
```shell
Logging in to [iface: default, target: iqn.2023-05.black.com:server, portal: 192.168.100.179,3260]
iscsiadm: Could not login to [iface: default, target: iqn.2023-05.black.com:server, portal: 192.168.100.179,3260].
iscsiadm: initiator reported error (24 - iSCSI login failed due to authorization failure)
iscsiadm: Could not log into all portals

认证方式一致，账号密码不对, 修改过后需要重新发现
```
