## 服务端安装
```shell
# 安装后默认启动
sudo apt install tgt 
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
sudo nvim /etc/tgt/conf.d/iscsi.cnf

# 定义LUN（逻辑单元号）的名称。
<target iqn.2023-05.black.com:lun1>
    # 定义了iSCSI Target服务器上存储设备的位置和名称（可以是物理磁盘或者LVM）
    # 注意：使用的存储对象必须是新建的，而不能是在用的。
    backing-store /dev/sdb
    # 定义iSCSI启动器的IP地址——ACL
    initiator-address 192.168.2.77
    # initiator-address 192.168.2.0/24
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

# 客户端发现服务端 target
sudo iscsiadm -m discovery -t st -p 192.168.2.77

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
