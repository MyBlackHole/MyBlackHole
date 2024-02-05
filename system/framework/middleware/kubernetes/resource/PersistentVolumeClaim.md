# PersistentVolumeClaim

[细节](https://kubernetes.io/zh-cn/docs/concepts/storage/persistent-volumes/)

1. 集群管理员预先创建存储类（StorageClass）；

2. 用户创建使用存储类的持久化存储声明(PVC：PersistentVolumeClaim)；

3. 存储持久化声明通知系统，它需要一个持久化存储(PV: PersistentVolume)；

4. 系统读取存储类的信息；

5. 系统基于存储类的信息，在后台自动创建PVC需要的PV；

6. 用户创建一个使用PVC的Pod；

7. Pod中的应用通过PVC进行数据的持久化；

8. 而PVC使用PV进行数据的最终持久化处理。
