# kubectl

Kubectl是kubenetes命令行工具，通过kubectl可以部署和管理应用，查看各种资源，创建，删除和更新组件 

kubectl command type name flags

command：指定要对资源执行的操作，比如create、get、delete。 
type：指定资源的类型，比如deployment、pod、service。 
name：指定资源的名称，名称大小写敏感。 
Flags：指定额外的可选参数。 

挂起（Pending）：Pod 已被 Kubernetes 系统接受，但有一个或者多个容器镜像尚未创建。等待时间包括调度 Pod 的时间和通过网络下载镜像的时间，这可能需要花点时间。  
运行中（Running）：该 Pod 已经绑定到了一个节点上，Pod 中所有的容器都已被创建。至少有一个容器正在运行，或者正处于启动或重启状态。  
成功（Succeeded）：Pod 中的所有容器都被成功终止，并且不会再重启。 失败（Failed）：Pod 中的所有容器都已终止了，并且至少有一个容器是因为失败终止。也就是说，容器以非0状态退出或者被系统终止。  
未知（Unknown）：因为某些原因无法取得 Pod 的状态，通常是因为与 Pod 所在主机通信失败。

## config

|   |   |
|---|---|
|kubectl config view|查看当前上下文|
|kubectl config set-credentials soso --client-certificate=soso.crt --client-key=soso.key --embed-certs=true <br><br>kubectl config set-context soso@kubernetes --cluster=kubernetes --user=soso|生成账号 <br><br>设置上下文环境--指的是这个账号只能在这个环境中才能用|
|kubectl  config use-context soso@kubernetes|切换账号|

## run

创建并运行一个或多个容器镜像 

创建一个deployment 或job 来管理容器

- 语法
run NAME --image=image [--env="key=value"] [--port=port] [--replicas=replicas] [--dry-run=bool] [--overrides=inline-json] [--command] -- [COMMAND] [args...]

|   |   |
|---|---|
|kubectl runnginx --image=nginx|启动nginx实例|
|kubectl runhazelcast --image=hazelcast --port=5701|启动hazelcast实例，暴露容器端口 5701|
|kubectl runhazelcast --image=hazelcast --env="DNS_DOMAIN=cluster"--env="POD_NAMESPACE=default"|启动hazelcast实例，在容器中设置环境变量“DNS_DOMAIN = cluster”和“POD_NAMESPACE = default”|
|kubectl runnginx --image=nginx --replicas=5|启动nginx实例，设置副本数5。|
|kubectl run nginx --image=nginx --dry-run|运行 Dry  打印相应的API对象而不创建它们|

## expose
将资源暴露为新的Kubernetes Service

- 语法
expose (-f FILENAME | TYPE NAME) [--port=port] [--protocol=TCP|UDP] [--target-port=number-or-name] [--name=name] [--external-ip=external-ip-of-service] [--type=type]

|   |   |
|---|---|
|kubectl expose rc nginx --port=80 --target-port=8000|为RC的nginx创建service，并通过Service的80端口转发至容器的8000端口上|
|kubectl expose -f nginx-controller.yaml --port=80 --target-port=8000|由“nginx-controller.yaml”中指定的type和name标识的RC创建Service，并通过Service的80端口转发至容器的8000端口上|



## annotate

更新一个或多个资源的Annotations信息

- 语法
annotate [--overwrite] (-f FILENAME | TYPE NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=version]

|   |   |
|---|---|
|kubectl annotate pods foo description='my frontend'|更新Pod“foo”，设置annotation “description”的value “my frontend”，如果同一个annotation多次设置，则只使用最后设置的value值|
|kubectl annotate -f pod.json description='my frontend'|根据“pod.json”中的type和name更新pod的annotation|
|kubectl annotate --overwrite pods foo description='my frontend running nginx'|更新Pod"foo"，设置annotation“description”的value“my frontend running nginx”，覆盖现有的值|
|kubectl annotate pods --all description='my frontend running nginx'|更新 namespace中的所有pod|
|kubectl annotate pods foo description='my frontend running nginx' --resource-version=1|只有当resource-version为1时，才更新pod ' foo '|
|kubectl annotate pods foo description-|通过删除名为“description”的annotations来更新pod ' foo '。#不需要- overwrite flag|

## autoscale

使用 autoscaler 自动设置在kubernetes集群中运行的pod数量（水平自动伸缩）

- 语法
autoscale (-f FILENAME | TYPE NAME | TYPE/NAME) [--min=MINPODS] --max=MAXPODS [--cpu-percent=CPU] [flags]

|   |   |
|---|---|
|kubectl autoscale deployment foo --min=2 --max=10|使用 Deployment “foo”设定，使用默认的自动伸缩策略，指定目标CPU使用率，使其Pod数量在2到10之间|
|kubectl autoscale rc foo --max=5 --cpu-percent=80|使用RC“foo”设定，使其Pod的数量介于1和5之间，CPU使用率维持在80％|


## convert

转换配置文件为不同的API版本，支持YAML和JSON格式

- 语法
convert -f FILENAME

|   |   |
|---|---|
|kubectl convert -f pod.yaml|将“pod.yaml”转换为最新版本并打印到stdout|
|kubectl convert -f pod.yaml --local -o json|将“pod.yaml”指定的资源的实时状态转换为最新版本＃，并以json格式打印到stdout|
|kubectl convert -f . \| kubectl create -f -|将当前目录下的所有文件转换为最新版本，并将其全部创建|

## create

通过配置文件名或stdin创建一个集群资源对象
- 语法
create -f FILENAME

|   |   |
|---|---|
|kubectl create/patch -f nginx-pod.yaml|命令式对象配置：通过命令配置和配置文件去操作kubernetes的资源。|
|kubectl create deployment nginx --image=nginx:1.17.1 --dry-run=client -n dev -o yaml|使用kubectl create命令生成yaml文件|
|kubectl create -f ./pod.json|通过pod.json文件创建一个pod|
|cat pod.json \| kubectl create -f -|通过stdin的JSON创建一个pod|
|kubectl create -f docker-registry.yaml --edit --output-version=v1 -o json|API版本为v1的JSON格式的docker-registry.yaml文件创建资源|

### clusterrole

创建一个ClusterRole
- 语法
clusterrole NAME --verb=verb --resource=resource.group [--resource-name=resourcename] [--dry-run]

|   |   |
|---|---|
|kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods|创建一个名为“pod-reader”的ClusterRole，允许用户在pod上执行“get”，“watch”和“list”|
|kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods --resource-name=readablepod --resource-name=anotherpod|创建一个名为“pod-reader”的ClusterRole，其中指定了ResourceName|
|kubectl create clusterrole foo --verb=get,list,watch --resource=rs.extensions|在指定的API Group中创建为"foo"的ClusterRole|
|kubectl create clusterrole foo --verb=get,list,watch --resource=pods,pods/status|创建一个名为“foo”的ClusterRole，并指定SubResource|
|kubectl create clusterrole "foo" --verb=get --non-resource-url=/logs/* |使用指定的NonResourceURL创建名称“foo”的ClusterRole|
### clusterrolebinding

为特定的ClusterRole创建ClusterRoleBinding
- 语法
clusterrolebinding NAME --clusterrole=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run]

|   |   |
|---|---|
|kubectl create clusterrolebinding cluster-admin --clusterrole=cluster-admin --user=user1 --user=user2 --group=group1|在集群范围将cluster-admin ClusterRole授予用户user1，user2和group1|


### configmap

根据配置文件、目录或指定的literal-value创建configmap
- 语法
configmap NAME [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run]

|   |   |
|---|---|
|kubectl create configmap my-config --from-file=path/to/bar|根据文件创建一个名为my-config的configmap|
|kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt --from-file=key2=/path/to/bar/file2.txt|使用指定的keys创建一个名为my-config的configmap|
|kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2|使用key1 = config1和key2 = config2创建一个名为my-config的configmap|
|kubectl create configmap my-config --from-file=path/to/bar|从文件中的key = value对创建一个名为my-config的configmap|
|kubectl create configmap my-config --from-env-file=path/to/bar.env|从env文件创建一个名为my-config的configmap|

### deployment

创建具有指定名称的deployment
- 语法
deployment NAME --image=image [--dry-run]

|   |   |
|---|---|
|kubectl create deployment nginx --image=nginx "or" kubectl expose deployment nginx --port=80 --type=NodePort|Kubernetes集群中部署一个Nginx|
|kubectl create deployment my-dep --image=busybox|创建一个名为my-dep的deployment，运行busybox镜像|



## delete 
通过配置文件名、stdin、资源名称或label选择器来删除资源
- 语法
delete ([-f FILENAME] | TYPE [(NAME | -l label | --all)])

|   |   |
|---|---|
|kubectl delete -f ./pod.json|使用 pod.json中指定的资源类型和名称删除pod|
|cat pod.json \| kubectl delete -f -|根据传入stdin的JSON所指定的类型和名称删除pod|
|kubectl delete pod,service baz foo|删除名为“baz”和“foo”的Pod和Service|
|kubectl delete pods,services -l name=myLabel|删除 Label name = myLabel的pod和Service|
|kubectl delete pod foo --grace-period=0 --force|强制删除dead node上的pod|
|kubectl delete pods --all|删除所有pod|
|   |   |
|---|---|
|kubectl delete deployment nginx|删除nginx的控制器|
|   |   |
|---|---|
|kubectl delete namespace mem-example|删除命名空间。命令会删除你根据这个任务创建的所有 Pod|
|   |   |
|---|---|
|kubectl delete pod nginx-xxx|删除nginx-xxxx的pod|
|   |   |
|---|---|
|kubectl delete service nginx-xxx|删除nginx-xxxx的service|


## 例子
- 查看服务日志
```shell
journalctl -xeu kubelet
```
