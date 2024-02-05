# redis ha

主从

```shell
helm repo add stable http://mirror.azure.cn/kubernetes/charts/


helm search repo redis
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION
stable/prometheus-redis-exporter        3.5.1           1.3.4           DEPRECATED Prometheus exporter for Redis metrics
stable/redis                            10.5.7          5.0.7           DEPRECATED Open source, advanced key-value stor...
stable/redis-ha                         4.4.6           5.0.6           DEPRECATED - Highly available Kubernetes implem...
stable/sensu                            0.2.5           0.28            DEPRECATED Sensu monitoring framework backed by...

helm pull stable/redis-ha

tar zxvf redis-ha-4.4.6.tgz

<!-- 模拟渲染 -->
helm template --debug ./redis-ha > all.yaml

kubectl create namespace redis

helm install redis ./redis-ha -n redis
```
