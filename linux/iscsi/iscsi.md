# iscsi
iSCSI是由IBM发明的基于以太网的存储协议
该协议与SUN的NFS协议都是为了解决存储资源共享问题的解决方案
两者意图一致，只不过两者是不同的实现方式，前者在客户机上呈现的是一个块设备，而后者则是一个目录树
关于两者的区别，可以参考本号之前的文章，本位不再赘述。

- 概述
概括的说，iSCSI是一种存储设备远程映射技术
它可以将一个远程服务器上的存储设备映射到本地
并呈现为一个块设备（大白话就是磁盘）
从普通用户的角度，映射过来的磁盘与本地安装的磁盘毫无差异。


## 基本概念介绍
- ACL(access control list)
    访问控制列表
- Network Portal
     网络端口。网络实体的一个组成部分，它有一个 TCP/IP 地址。 网络端口在 initiator 用 IP 地址标识， 在 target 用 IP 地址＋侦听的 TCP 端口标识。
- Session
     连接 initiator 和 target 的一组 TCP 连接构成一个 session(可以简单理解为 I_T nexus)。可以向 session 添加 TCP 连接，也可以把 TCP 连接从 session 删除。 也就是说一个session中是可以有多个连接的。通过一个 session 的所有连接，initiator 只看到同一个 target。
- Connection 
     一个 TCP 连接。Initiator 和 target 之间使用一或者多个 TCP 连接通信。
- CID(Connection ID)
     一个 session 里的每个 connection 用 CID 进行标识，该标识在 session 范围内是唯一。CID 由 initiator 产生，在 login 请求和使用 logout 关闭 连接时传递给 target。
- SSID（Session ID）
    一个 iSCSI Initiator 与 iSCSI Target 之间的会话（Session）由会话ID（SSID）定义，该会话ID是一个由发起方部分（ISID）和目标部分（Target Portal Group Tag）组成的元组。 ISID 在会话建立时由发起者明确指定。 Target Portal Group Tag 由发起者在连接建立时选择的 TCP端口来隐式指定。 当给定 TargetName 时，TargetPortalGroupTag 也必须由目标在连接建立期间作为确认返回。
- Portal Groups
     网络端口组。iSCSI session 支持多连接，一些实现能把通过多个端口建立的多个连接捆绑到一个 session。 一个 iSCSI 网络实体的多个网络端口被定义为一个网络端口组，把该组和一个 session 联系起来，该 session 就可以捆绑通过该组内多个端口建立的多个连接，再使它们一起协同工作以达到捆绑的目的。每一个该组的 session 并不需要包括该组的所有网络端口。一个 iSCSI 节点可能有一或者多个网络端口组，但是每一个 iSCSI 使用的网络端口只能属于 iSCSI 节点的一个组。
- Target Portal Group Tag
     网络端口组标识。使用 16 比特的数标识一个网络端口组。在 一个 iSCSI 节点里，所有具有同样组标志的端口构成一个网络端口组。
- iSCSI Task
     一个 iSCSI 任务是指一个需要响应的 iSCSI 请求。
- I_T nexus
     I_T nexus 是指一个 SCSI initiator 的端口和一个 SCSI target 端口之间 的关系。 对于 iSCSI， 这个关系对应一个 session， 它指 session 的 initiator 端和 iSCSI target 网络端口组之间的关系。I_T nexus 的标识是一对端口名称(iSCSI initiator 名称＋i＋ISID，iSCSI target 名称＋t＋网络端口组标识)。 PDU (Protocol Data Unit)
     initiator 和 target 之间通信时把信息分割为消息。这些 消息称为 iSCSI PDU。 SSID (Session ID)
     iSCSI initiator 和 iSCSI target 之间的 session 用 SSID 进行标识， 该标识由 initiator 部分的 ISID 和 target 部分的 TPGT 构成。
- ISID（The initiator part of the Session Identifier）
    发起方会话标识，由 initiator 在 session 建立的时候明确给出，
- TSIH (Target Session Identifying Handle)
     Target 分配给与特定名称 initiator 建立的 session 的标识。 但是 0 值被保留着用于 initiator 告知 target 这是一个新 session。 在为一个 session 添加一个 connect 时，TSIH 已经隐含指明。

## 服务端安装
```shell
sudo apt install tgt # 安装后默认启动
systemctl status tgt
```

### 配置
iscsi 命名方式 iqn、EUI
- iqn
iqn.<YYYY-MM>.<reversed domain name>:<extra-name>

```shell
例如：
    iqn.2015-08.example.com:disk0

# 编辑
vim /etc/tgt/conf.d/iscsi.cnf

# 定义LUN（逻辑单元号）的名称。
<target iqn.2021-03.bee.com:lun1>
    # 定义了iSCSI Target服务器上存储设备的位置和名称（可以是物理磁盘或者LVM）
    # 注意：使用的存储对象必须是新建的，而不能是在用的。
    backing-store /dev/sdb
    # 定义iSCSI启动器的IP地址——ACL
    initiator-address 192.168.91.152
    # initiator-address 192.168.91.0/24
    # 定义传入的用户名/密码 iscsi-user password
    incominguser test01 123456
    # 定义目标将提供给启动器的用户名/密码 iscsi-target secretpass
    outgoinguser test02 654321
</target>

systemctl restart tgt
sudo tgtadm --mode target --op show
```

## 客户端
```shell
sudo apt install open-iscsi

# 客户端发现服务端target
sudo iscsiadm -m discovery -t st -p 192.168.106.66

# out:
192.168.106.66:3260,1 iqn.2023-03.bee.com:lun1
```

### 配置
节点配置文件将存放于目录 /etc/iscsi/nodes/ 中，并且每个LUN都有一个对应的配置目录。
比如：/etc/iscsi/nodes/iqn.2021-03.bee.com:iscsi.disk0/192.168.91.151,3260,1/default
在上述发现命令执行完毕后将在 /etc/iscsi/nodes/ 中自动生成指向iscsi target的IP的配置目录。

如果要更新服务端target的配置需要将 /etc/iscsi/nodes/ 下的配置目录删除，然后再执行iscsiadm -m discovery … 发现命令，以生成新的配置。

```shell
# 添加iSCSI Target LUN名称
sudo nvim /etc/iscsi/initiatorname.iscsi
# 注意InitiatorName只能有一个。主要用于标识Initiator，与target无关。
InitiatorName=iqn.2021-03.bee.com:lun1.init1

# 定义Initiator对应iscsi target的CHAP认证信息（可选）
sudo nvim /etc/iscsi/iscsid.conf

## 通过配置 修改以下信息
node.session.auth.authmethod = CHAP
node.session.auth.username = test01 # incominguser
node.session.auth.password = 123456 # incominguser
node.session.auth.username_in = test02 # outgoinguser
node.session.auth.password_in = 654321 # outgoinguser
node.startup = automatic # 开机自动登陆iscsi target（必选）

## 通过命令 修改
iscsiadm -m node -T iqn.2023-03.bee.com:lun1 -p 192.168.106.66:3260 --op update -n node.session.auth.authmethod -v CHAP
iscsiadm -m node -T iqn.2023-03.bee.com:lun1 -p 192.168.106.66:3260 --op update -n node.startup -v automatic
```

## 测试
```shell
# 这里会自动登陆iscsi target（更新配置时的出错考虑删除/etc/iscsi/nodes下的配置文件夹），完了使用iscsiadm -m node -o show 查看生成的配置。
systemctl restart open-iscsi iscsid

# 查看iSCSI Initiator工作状态
systemctl status open-iscsi
iscsiadm -m session -o show

# 发现iscsi target
iscsiadm -m discovery -t sendtargets -p 192.168.106.66
或者
iscsiadm -m node --login

# 登陆iscsi target
iscsiadm -m node -T iqn.2023-03.bee.com:lun1 -p 192.168.106.66 -l
out:
Logging in to [iface: default, target: iqn.2023-03.bee.com:lun1, portal: 192.168.106.66,3260]
Login to [iface: default, target: iqn.2023-03.bee.com:lun1, portal: 192.168.106.66,3260] successful.

# 登出iscsi target
iscsiadm -m node -T iqn.2023-03.bee.com:lun1 -p 192.168.106.66 -u
out:
Logging out of session [sid: 1, target: iqn.2023-03.bee.com:lun1, portal: 192.168.106.66,3260]
Logout of [sid: 1, target: iqn.2023-03.bee.com:lun1, portal: 192.168.106.66,3260] successful.

# 查看LUN设备
fdisk -l
cat /proc/partitions
lsblk
# 查看UUID
blkid
```

- 退出所有登陆的目标器
```shell
iscsiadm -m node -U all

# 连接死掉或者断网
iscsiadm -m node -o delete –T  iqn.2015.06.cn.hrbyg.www.ygcs.c0a802b8:wzgchap -p 192.168.14.112
```
