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
     I_T nexus 是指一个 SCSI initiator 的端口和一个 SCSI target 端口之间 的关系。 对于 iSCSI， 这个关系对应一个 session， 它指 session 的 initiator 端和 iSCSI target 网络端口组之间的关系。I_T nexus 的标识是一对端口名称(iSCSI initiator 名称＋i＋ISID，iSCSI target 名称＋t＋网络端口组标识)。 
     initiator 和 target 之间通信时把信息分割为消息。这些 消息称为 iSCSI PDU。 SSID (Session ID)
     iSCSI initiator 和 iSCSI target 之间的 session 用 SSID 进行标识， 该标识由 initiator 部分的 ISID 和 target 部分的 TPGT 构成。
- ISID（The initiator part of the Session Identifier）
    发起方会话标识，由 initiator 在 session 建立的时候明确给出，
- TSIH (Target Session Identifying Handle)
     Target 分配给与特定名称 initiator 建立的 session 的标识。 但是 0 值被保留着用于 initiator 告知 target 这是一个新 session。 在为一个 session 添加一个 connect 时，TSIH 已经隐含指明。

- PDU
    PDU (Protocol Data Unit)

![[imgs/Pasted image 20230715092732.png]]