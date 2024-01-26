# source

## 入口
- server-main

## 对象层设计
- erasureServerPools(纠错对象池)
 - erasureSets(纠错对象集)
  - erasureObjects(纠错对象)

## 存储层(StorageAPI)

- xlStorageDiskIDCheck(本地磁盘)
 - xlStorage
- storageRESTClient(同等节点磁盘)

每个 erasureSets 可以共用 xlStorageDiskIDCheck
可以一个 erasureObjects 对应多个 xlStorageDiskIDCheck

storageRESTServer
