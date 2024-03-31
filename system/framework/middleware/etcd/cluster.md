# cluster

- 拷贝三份
```shell
[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
WorkingDirectory=/var/lib/etcd/
EnvironmentFile=-/etc/etcd/etcd1.conf
User=etcd
# set GOMAXPROCS to number of processors
ExecStart=/bin/bash -c "GOMAXPROCS=$(nproc) /usr/bin/etcd --name=\"${ETCD_NAME}\" --data-dir=\"${ETCD_DATA_DIR}\" --listen-client-urls=\"${ETCD_LISTEN_CLIENT_URLS}\""
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

- etcd1.conf
```shell
ETCD_DATA_DIR="/data/k8s/etcd/data"
ETCD_WAL_DIR="/data/k8s/etcd/wal"
ETCD_LISTEN_PEER_URLS="http://192.168.1.85:2380"
ETCD_LISTEN_CLIENT_URLS="http://192.168.1.85:2379"
ETCD_MAX_SNAPSHOTS="5"
ETCD_MAX_WALS="5"
ETCD_NAME="etcd1"
ETCD_SNAPSHOT_COUNT="100000"
ETCD_HEARTBEAT_INTERVAL="100"
ETCD_ELECTION_TIMEOUT="1000"

ETCD_INITIAL_ADVERTISE_PEER_URLS="http://192.168.1.85:2380"
ETCD_ADVERTISE_CLIENT_URLS="http://192.168.1.85:2379"

ETCD_INITIAL_CLUSTER="etcd1=http://192.168.1.85:2380,etcd2=http://192.168.1.86:2380,etcd3=http://192.168.1.87:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"
```

- etcd2.conf
```shell
ETCD_DATA_DIR="/data/k8s/etcd/data"
ETCD_WAL_DIR="/data/k8s/etcd/wal"
ETCD_LISTEN_PEER_URLS="http://192.168.1.86:2380"
ETCD_LISTEN_CLIENT_URLS="http://192.168.1.86:2379"
ETCD_MAX_SNAPSHOTS="5"
ETCD_MAX_WALS="5"
ETCD_NAME="etcd2"
ETCD_SNAPSHOT_COUNT="100000"
ETCD_HEARTBEAT_INTERVAL="100"
ETCD_ELECTION_TIMEOUT="1000"

ETCD_INITIAL_ADVERTISE_PEER_URLS="http://192.168.1.86:2380"
ETCD_ADVERTISE_CLIENT_URLS="http://192.168.1.86:2379"

ETCD_INITIAL_CLUSTER="etcd1=http://192.168.1.85:2380,etcd2=http://192.168.1.86:2380,etcd3=http://192.168.1.87:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"
```

- etcd3.conf
```shell
ETCD_DATA_DIR="/data/k8s/etcd/data"
ETCD_WAL_DIR="/data/k8s/etcd/wal"
ETCD_LISTEN_PEER_URLS="http://192.168.1.87:2380"
ETCD_LISTEN_CLIENT_URLS="http://192.168.1.87:2379"
ETCD_MAX_SNAPSHOTS="5"
ETCD_MAX_WALS="5"
ETCD_NAME="etcd3"
ETCD_SNAPSHOT_COUNT="100000"
ETCD_HEARTBEAT_INTERVAL="100"
ETCD_ELECTION_TIMEOUT="1000"

ETCD_INITIAL_ADVERTISE_PEER_URLS="http://192.168.1.87:2380"
ETCD_ADVERTISE_CLIENT_URLS="http://192.168.1.87:2379"

ETCD_INITIAL_CLUSTER="etcd1=http://192.168.1.85:2380,etcd2=http://192.168.1.86:2380,etcd3=http://192.168.1.87:2380"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"
```



- yaml 方式
```shell
<!-- etcd21 -->
name: etcd21
data-dir: /var/log/etcd_cluster02/data
listen-client-urls: http://192.168.60.218:3379
advertise-client-urls: http://192.168.60.218:3379
listen-peer-urls: http://192.168.60.218:3380
initial-advertise-peer-urls: http://192.168.60.218:3380
initial-cluster: etcd21=http://192.168.60.218:3380,etcd22=http://192.168.60.219:3380,etcd23=http://192.168.60.220:3380
initial-cluster-token: etcd-cluster-2
initial-cluster-state: new



name: etcd22
data-dir: /var/log/etcd_cluster02/data
listen-client-urls: http://192.168.60.219:3379
advertise-client-urls: http://192.168.60.219:3379
listen-peer-urls: http://192.168.60.219:3380
initial-advertise-peer-urls: http://192.168.60.219:3380
initial-cluster: etcd21=http://192.168.60.218:3380,etcd22=http://192.168.60.219:3380,etcd23=http://192.168.60.220:3380
initial-cluster-token: etcd-cluster-2
initial-cluster-state: new


name: etcd23
data-dir: /var/log/etcd_cluster02/data
listen-client-urls: http://192.168.60.220:3379
advertise-client-urls: http://192.168.60.220:3379
listen-peer-urls: http://192.168.60.220:3380
initial-advertise-peer-urls: http://192.168.60.220:3380
initial-cluster: etcd21=http://192.168.60.218:3380,etcd22=http://192.168.60.219:3380,etcd23=http://192.168.60.220:3380
initial-cluster-token: etcd-cluster-2
initial-cluster-state: new
```
