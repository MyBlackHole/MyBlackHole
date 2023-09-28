# kubernetes

## master

|                         |                                                                                                                                                            |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| kube-apiserver          | kubernetes api，集群的统一入口，各组件之间的协调者，以restful api提供接口服务，所有对象资源的增删改查和监听操作都交给apiserver处理后在提交给etcd存储记录； |
| kube-controller-manager | 处理集群中常规的后台任务，一种资源对应一个控制器，controller-manager就是负责管理这些控制器的；                                                             |
| kube-scheduler          | 根据调度算法为新创建的pod选择一个node节点，可以任意部署，可以部署在同一个节点上，也可以部署在不同节点上；                                                  |
| etcd                    | 分布式键值存储系统，用户保存集群状态数据，比如pod、service等对象信息；                                                                                     |

## node


|   |   |
|---|---|
|kubelet|kubelet时master在node节点上的代理agent，管理本node运行容器的生命周期，比如创建容器、pod挂载数据卷、下载sercet、获取容器和节点状态等工作，kubelet将每个pod转换成一组容器；|
|kube-proxy|在node节点上实现pod的网络代理，维护网络规则和四层的负载均衡工作；|
|docker|容器引擎，运行容器；|


## 概念

### pod
最小部署单元； 
一组容器的集合； 
一个pod中的容器共享网络命名空间； 
pod是短暂的；

### controllers

|   |   |
|---|---|
|replicaset|确保预期的pod副本数量|
|deployment|无状态应用部署，比如nginx、apache，一定程度上的增减不会影响客户体验；|
|statefulset|有状态应用部署，是独一无二型的，会影响到客户的体验；|
|daemonset|确保所有node运行同一个pod，确保pod在统一命名空间；|
|job|一次性任务|
|cronjob|定时任务；|

### service

防止pod失联； 
定义一组pod的访问策略； 
确保了每个pod的独立性和安全性；


### pollcies

策略


### ouths

|   |   |
|---|---|
|label|标签，附加到某个资源上，用户关联对象、查询和筛选；|
|namespaces|命名空间，将对象从逻辑上隔离；|
|annotations|注释；|




### 命令式对象管理

|   |   |   |   |   |
|---|---|---|---|---|
|类型|操作|适用场景|优点|缺点|
|命令式对象管理|对象|测试|简单|只能操作活动对象，无法审计、跟踪|

#### 例子

|   |   |
|---|---|
|kubectl create namespace dev|创建一个 命名空间 dev|
|kubectl get namespace|获取namespace|
|kubectl run nginx --image=nginx:1.17.1 -n dev|在刚才创建的namespace下创建并运行一个Nginx的Pod|
|kubectl get pods -n dev|查看名为dev的namespace下的所有Pod，如果不加-n，默认就是default的namespace|
|kubectl delete pod nginx -n dev|删除指定namespace下的指定Pod|
|kubectl delete namespace dev|删除指定的namespace|

### 命令式对象配置

|   |   |   |   |   |
|---|---|---|---|---|
|命令式对象配置|文件|开发|可以审计、跟踪|项目大的时候，配置文件多，操作麻烦|

#### 例子

|   |   |
|---|---|
|执行create命令，创建资源|kubectl create/patch -f nginx-pod.yaml|
|执行get命令，查看资源|kubectl get -f nginxpod.yaml|
|执行delete命令，删除资源|kubectl delete -f nginxpod.yaml|


- 推荐方式
创建和更新资源使用声明式对象配置：kubectl apply -f xxx.yaml。 
删除资源使用命令式对象配置：kubectl delete -f xxx.yaml。 
查询资源使用命令式对象管理：kubectl get(describe) 资源名称



