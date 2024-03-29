# rook

是一个开源的云原生存储编排工具，提供平台、框架和对各种存储解决方案的支持，以和云原生环境进行本地集成。
分布式存储
Rook 将存储软件转变成自我管理、自我扩展和自我修复的存储服务，通过自动化部署、启动、配置、供应、扩展、升级、迁移、灾难恢复、监控和资源管理来实现。Rook 底层使用云原生容器管理、调度和编排平台提供的能力来提供这些功能。
Rook 利用扩展功能将其深度地集成到云原生环境中，并为调度、生命周期管理、资源管理、安全性、监控等提供了无缝的体验。有关 Rook 当前支持的存储解决方案的状态相关的更多详细信息，可以参考 Rook 仓库 的项目介绍。Rook 目前支持 Ceph、NFS、Minio Object Store 和 CockroachDB。
[细节](https://cloud.tencent.com/developer/article/2242349)


## 例子
- 自动发现新存储设备
```shell
kubectl -n rook-ceph edit configmaps rook-ceph-operator-config

ROOK_ENABLE_DISCOVERY_DAEMON:⋅"true"↴
```
