# proxy

http://[k8s-master]:8009/api/v1/namespaces/[namespace-name]/services/[service-name]/proxy

- 临时暴露 k8s 内部服务
```shell
kubectl proxy --address=0.0.0.0 --port=8080

http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/kubernetes-dashboard-web/

http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard-web:/proxy/
```

