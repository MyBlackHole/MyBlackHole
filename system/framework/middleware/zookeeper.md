# zookeeper

## 安装
### docker
```shell
# 单点
## docker 拉取镜像同时后台启动 zookeeper server
docker run --name some-zookeeper --restart always -d zookeeper
<!-- ## 启动 app 连接到上一个容器 -->
<!-- docker run --name some-app --link some-zookeeper:zookeeper -d application-that-uses-zookeeper -->

## docker client 端, 连接到容器名 some-zookeeper 的 zookeeper
docker run -it --rm --link some-zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper
### 或
go install github.com/let-us-go/zkcli
zkcli -s "192.168.90.204:10001"

# client
docker run -it --rm --name zkclient zookeeper zkCli.sh -server 192.168.90.207:2181

## 可视化工具
wget https://github.com/vran-dev/PrettyZoo/releases/download/v2.1.1/prettyzoo_2.1.1_amd64.deb
```

### docker-compose
```docker-compose
version: '3.1'

services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zoo1
    ports:
      - 2181:2181
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo2:
    image: zookeeper
    restart: always
    hostname: zoo2
    ports:
      - 2182:2181
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo3:
    image: zookeeper
    restart: always
    hostname: zoo3
    ports:
      - 2183:2181
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181
```

## 命令
### ls
ls [-s] [-w] [-R] path
-s：查看节点状态（其效果和stat命令一样）
-w：监听子节点变化
-R：查看所有节点

### stat
查看节点状态
stat [-w] path
stat / 等价于 ls -s /

### create
添加节点
create [-s] [-e] [-c] [-t ttl] path [data] [acl]
-s：有序节点
-e：临时节点
-c：容器的节点类型
-t ttl：TTL节点，设置节点存活时间
path：指定要创建节点的路径
data：节点存储的数据
acl：访问权限相关，默认是 world（关于这部分的使用，请继续往下关注ACL部分的内容）

### get
查找节点
get [-s] [-w] path
-s：查看节点状态（其效果和stat命令一样）
-w：监听子节点变化

### set
修改节点
set [-s] [-v version] path data
-s：查看节点状态（其效果和stat命令一样）
-v version：当前更新操作的版本号；用于乐观锁，每次更新都根据 version 判断版本

### delete
删除节点
delete [-v version] path
-v version：和 set 命令相似，-v 参数用于判断当前操作的版本

### getAcl\setAcl
增删改查节点权限
对于节点 ACL 权限控制，是通过使用：scheme:id:perm 来标识（也就是例子中的格式 -> world:anyone:crwda）
其含义是：
- 权限模式（Scheme）：授权的策略
    - world：默认方式，相当于所有人都能访问
    - auth：授权的用户才能访问
    - digest：账号密码都正确鉴权的用户才能访问
    - ip：指定某ip的才能访问
- 授权对象（ID）:授权的对象
    - IP：具体的 IP 地址或 IP 段
    - World：只有“anyOne”这一个Id
    - Digest：自定义，格式为 username:BASE64(SHA-1(username:password))
- 权限（Permission）：授予的权限
    - CREATE： c 可以创建子节点
    - DELETE： d 可以删除子节点（仅下一级节点）
    - READ： r 可以读取节点数据及显示子节点列表
    - WRITE： w 可以设置节点数据
    - ADMIN： a 可以设置节点访问控制列表权限

举个例子：create /testAcl demo world:anyone:crwda
创建 testAcl 这个节点，节点值为demo，其权限策略是所有人都可以执行 crwda 操作

## 例子
- 先查询节点版本号，模拟并发下修改同一节点
```shell
[zk: zookeeper(CONNECTED) 12] get -s /a
10
cZxid = 0x3
ctime = Mon Apr 10 02:10:12 UTC 2023
mZxid = 0x12
mtime = Mon Apr 10 03:12:50 UTC 2023
pZxid = 0x3
cversion = 0
dataVersion = 5
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 2
numChildren = 0
# 客户端1
set -v 5 /a demo-data1

# 客户端2
set -v 5 /demo demo-data2

# 客户端1比客户端2先执行，客户端2再执行的话，这时显示报错
```

