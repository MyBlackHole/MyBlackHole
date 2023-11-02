# StatefulSet

[细节](https://kubernetes.io/zh-cn/docs/concepts/workloads/controllers/statefulset/)

- 稳定的、唯一的网络标识符。
- 稳定的、持久的存储。
- 有序的、优雅的部署和扩缩。
- 有序的、自动的滚动更新。

StatefulSet就像是一种特殊的Deployment，它使用Kubernetes里的两个标准功能：Headless Service 和 PVC，实现了对的拓扑状态和存储状态的维护。

StatefulSet通过Headless Service ， 为它管控的每个Pod创建了一个固定保持不变的DNS域名，来作为Pod在集群内的网络标识。加上为Pod进行编号并严格按照编号顺序进行Pod调度，这些机制保证了StatefulSet对维护应用拓扑状态的支持。

而借由StatefulSet定义文件中的volumeClaimTemplates声明Pod使用的PVC，它创建出来的PVC会以名称编号这些约定与它创建出来的Pod进行绑定，借由PVC独立于Pod的生命周期和两者之间的绑定机制的帮助，StatefulSet完成了应用存储状态的维护


## persistentVolumeClaimRetentionPolicy
 在 1.23 以后，有可选 .spec.persistentVolumeClaimRetentionPolicy 字段控制在 StatefulSet 的生命周期中是否保留或者删除 PVC。
  您必须启用 StatefulSetAutoDeletePVC feature gate 才能使用此字段。启用后，您可以为每个 StatefulSet 配置两个策略：

whenDeleted：配置删除 StatefulSet 时应用的卷保留行为。
whenScaled：配置当 StatefulSet 的副本数减少时应用的卷保留行为。
对于上面两个策略，可以将值设置为 Delete 或 Retain。

Delete：对于受策略影响的每个Pod，将删除从 StatefulSet volumeClaimTemplate 创建的PVC。使用 whenDeleted 策略，volumeClaimTemplate 中的所有PVC 将在其 Pod 被删除后被删除。使用 whenScaled 策略，在删除 Pod 副本后，仅删除与正在缩小的 Pod 副本相对应的PVC。
Retain（默认）：volumeClaimTemplate 中的 PVC 在其 Pod 被删除时不受影响。1.23 之前版本也是这样的行为。
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: rabbitmq
  namespace: rabbitmq
spec:
  persistentVolumeClaimRetentionPolicy:
    whenDeleted: Delete
    whenScaled: Delete
```
