# helm

[细节](https://helm.sh/zh/docs/helm/helm/)


```shell
helm version                            // 查看helm版本
helm create xxx                         // 创建一个xxx charts
helm lint ./xxx                         // 检查包的格式或信息是否有问题
helm list                               // 列出已经部署的charts
helm history                            // 发布历史
helm upgrade                            // 更新版本
helm rollback                           // 回滚版本
helm package ./xxx                      // 打包charts
helm repo add --username admin --password password myharbor xxx  // 增加repo
helm uninstall xxx1                     // 卸载删除xxx1
helm pull                                // 拉取chart包
helm cm-push                            // 推送chart包
helm repo update                        // 更新仓库资源
helm search hub                         // 从 Artifact Hub 中查找并列出 helm charts。 Artifact Hub中存放了大量不同的仓库
helm search repo                        // 从你添加（使用 helm repo add）到本地 helm 客户端中的仓库中进行查找。该命令基于本地数据进行搜索，无需连接互联网
```

