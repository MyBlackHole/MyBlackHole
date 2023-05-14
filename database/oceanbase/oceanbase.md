# oceanbase

## install

### all-in-one
```shell
bash -c "$(curl -s https://obbusiness-private.oss-cn-shanghai.aliyuncs.com/download-center/opensource/oceanbase-all-in-one/installer.sh)"
source 
```

### docker
```shell
docker pull oceanbase/oceanbase-ce


# 根据当前容器部署最大规格实例
docker run -p 2881:2881 -v $PWD/ob:/root/ob -v $PWD/obd:/root/.obd --name obstandalone -e MINI_MODE=0 -d oceanbase/oceanbase-ce

# 部署 mini 的独立实例
docker run -p 2881:2881 -v $PWD/ob:/root/ob -v $PWD/obd:/root/.obd --name obstandalone -e MINI_MODE=1 -d oceanbase/oceanbase-ce

# 修改配置, (处理:[ERROR] OBD-1006: Failed to connect to obagent)
lvim obd/cluster/obcluster/config.yaml
:%s/127\.0\.0\.1/localhost/g

# 验证
docker logs obstandalone | tail -1
boot success!

# 连接
## 使用 root 用户登录集群的 sys 租户
docker exec -it obstandalone ob-mysql sys

## 使用 root 用户登录集群的 test 租户
docker exec -it obstandalone ob-mysql root

## 使用 test 用户登录集群的 test 租户
docker exec -it obstandalone ob-mysql test
```
