# kubesphere

- 最小化处理
```shell
kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.3.0/kubesphere-installer.yaml
kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.3.0/cluster-configuration.yaml

<!-- 安装进度 -->
kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-installer -o jsonpath='{.items[0].metadata.name}') -f
```

```shell
export KKZONE=cn

curl -sfL https://get-kk.kubesphere.io | VERSION=v3.0.7 sh -

chmod +x kk

./kk create config

<!-- 修改配置(config.md) -->

./kk create cluster --with-kubesphere v3.4.0


#####################################################
###              Welcome to KubeSphere!           ###
#####################################################

Console: http://192.168.78.212:30880
Account: admin
Password: P@88w0rd
NOTES：
  1. After you log into the console, please check the
     monitoring status of service components in
     "Cluster Management". If any service is not
     ready, please wait patiently until all components
     are up and running.
  2. Please change the default password after login.

#####################################################
https://kubesphere.io             2023-09-28 09:12:27
#####################################################
09:12:29 CST success: [node1]
09:12:29 CST Pipeline[CreateClusterPipeline] execute successfully
Installation is complete.

Please check the result using the command:

        kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l 'app in (ks-install, ks-installer)' -o jsonpath='{.items[0].metadata.name}') -f
```
