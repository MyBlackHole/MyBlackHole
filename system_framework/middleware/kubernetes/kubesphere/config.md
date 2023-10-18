# config
```shell
apiVersion: kubekey.kubesphere.io/v1alpha2
kind: Cluster
metadata:
  name: sample
spec:
  hosts:
  - {name: node1, address: 192.168.78.212, internalAddress: 192.168.78.212, user: root, password: "xxxxxx"}
  - {name: node2, address: 192.168.78.213, internalAddress: 192.168.78.213, user: root, password: "xxxxxx"}
  - {name: node3, address: 192.168.78.214, internalAddress: 192.168.78.214, user: root, password: "xxxxxx"}
  roleGroups:
    etcd:
    - node1
    control-plane:
    - node1
    worker:
    - node1
    - node2
    - node3
  controlPlaneEndpoint:
    ## Internal loadbalancer for apiservers
    # internalLoadbalancer: haproxy

    domain: lb.kubesphere.local
    address: ""
    port: 6443
  kubernetes:
    version: v1.23.10
    clusterName: cluster.local
    autoRenewCerts: true
    containerManager: docker
  etcd:
    type: kubekey
  network:
    plugin: calico
    kubePodsCIDR: 10.233.64.0/18
    kubeServiceCIDR: 10.233.0.0/18
    ## multus support. https://github.com/k8snetworkplumbingwg/multus-cni
    multusCNI:
      enabled: false
  registry:
    privateRegistry: ""
    namespaceOverride: ""
    registryMirrors: []
    insecureRegistries: []
  addons: []

```
