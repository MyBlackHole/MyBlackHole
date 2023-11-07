# install

```shell
helm install xxx1 ./xxx                 // 部署安装xxx，设置名称为xxx1
helm install [RELEASE_NAME] apisix/apisix --namespace ingress-apisix --create-namespace
helm install -f ./apisix/values.yaml apisix ./apisix --namespace ingress-apisix --create-namespace
```
