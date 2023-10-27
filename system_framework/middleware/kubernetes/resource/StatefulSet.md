# StatefulSet

[细节](https://kubernetes.io/zh-cn/docs/concepts/workloads/controllers/statefulset/)

- 稳定的、唯一的网络标识符。
- 稳定的、持久的存储。
- 有序的、优雅的部署和扩缩。
- 有序的、自动的滚动更新。

StatefulSet就像是一种特殊的Deployment，它使用Kubernetes里的两个标准功能：Headless Service 和 PVC，实现了对的拓扑状态和存储状态的维护。

StatefulSet通过Headless Service ， 为它管控的每个Pod创建了一个固定保持不变的DNS域名，来作为Pod在集群内的网络标识。加上为Pod进行编号并严格按照编号顺序进行Pod调度，这些机制保证了StatefulSet对维护应用拓扑状态的支持。

而借由StatefulSet定义文件中的volumeClaimTemplates声明Pod使用的PVC，它创建出来的PVC会以名称编号这些约定与它创建出来的Pod进行绑定，借由PVC独立于Pod的生命周期和两者之间的绑定机制的帮助，StatefulSet完成了应用存储状态的维护
