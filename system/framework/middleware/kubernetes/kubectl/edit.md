# edit


使用默认编辑器 编辑服务器上定义的资源
- 语法
edit (RESOURCE/NAME | -f FILENAME)

|   |   |
|---|---|
|kubectl edit svc/docker-registry|编辑名为'docker-registry'的service|
|KUBE_EDITOR="nano" kubectl edit svc/docker-registry|使用替代的编辑器|
|kubectl edit job.v1.batch/myjob -o json|编辑名为“myjob”的service，输出JSON格式 V1 API版本|
|kubectl edit deployment/mydeployment -o yaml --save-config|以YAML格式输出编辑deployment“mydeployment”，并将修改的配置保存在annotation中|


