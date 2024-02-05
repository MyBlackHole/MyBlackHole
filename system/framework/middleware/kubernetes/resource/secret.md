# secret


## cert-manager
![[imgs/Pasted image 20231020113847.png]]
```shell
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.1/cert-manager.yaml

kubectl get pods --namespace cert-manager

```
1. 创建自签名发行者
2. 生成CA证书
3. 创建CA发行者（ClusterIssuer）
4. 生成网站证书
5. 将网站证书配置到Ingress

### 创建自签名发行者
```shell
# selfsigned-issuer.issuer.yaml
# 参考：https://cert-manager.io/docs/configuration/selfsigned/
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: selfsigned-issuer
  namespace: cert-manager
spec:
  selfSigned: {}
```

### 生成CA证书
```shell
# ca-example-com.certificate.cert-manager.yaml
# 参考：https://cert-manager.io/docs/usage/certificate/
# api参考：https://cert-manager.io/docs/reference/api-docs/#cert-manager.io/v1alpha3.Certificate
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: ca-example-com ###
  namespace: cert-manager ### 修改为cert-manager的namespace，以让ClusterIssuer的CA Issuer可以使用此证书
spec:
  # Secret names are always required.
  secretName: ca-example-com-tls ### Secret名字
  duration: 2160h # 90d
  renewBefore: 360h # 15d
  subject:
    organizations:
    - Example Inc. ###
  # The use of the common name field has been deprecated since 2000 and is
  # discouraged from being used.
  commonName: ca.example.com ###
  isCA: true ### 修改为true，isCA将将此证书标记为对证书签名有效。这会将cert sign自动添加到usages列表中。
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048
  #usages: ### 注释了usages，使用情况是证书要求的x509使用情况的集合。默认为digital signature，key encipherment如果未指定。
  #  - server auth
  #  - client auth
  # At least one of a DNS Name, URI, or IP address is required.
  dnsNames:
  - ca.example.com ###
  #uris: ### 注释了uris、ipAddresses
  #- spiffe://cluster.local/ns/sandbox/sa/example
  #ipAddresses:
  #- 192.168.0.5
  # Issuer references are always required.
  issuerRef:
    name: selfsigned-issuer ### 指定为自签名发行人
    # We can reference ClusterIssuers by changing the kind here.
    # The default value is Issuer (i.e. a locally namespaced Issuer)
    kind: Issuer
    # This is optional since cert-manager will default to this value however
    # if you are using an external issuer, change this to that issuer group.
    group: cert-manager.io
```

### 创建CA发行者
```shell
# ca-issuer.clusterissuer.yaml
# 参考：https://cert-manager.io/docs/configuration/ca/
apiVersion: cert-manager.io/v1
kind: ClusterIssuer ### ClusterIssuer
metadata:
  name: ca-issuer
  namespace: cert-manager ### ClusterIssuer下namespace无效
spec:
  ca:
    secretName: ca-example-com-tls ###
```


### 生成网站证书
```shell
# site-example-com.certificate.example-com.yaml
# 参考：https://cert-manager.io/docs/usage/certificate/
# api参考：https://cert-manager.io/docs/reference/api-docs/#cert-manager.io/v1alpha3.Certificate
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: kubernetes-dashboard-certs ###
  namespace: kubernetes-dashboard ### 站点所在名字空间
spec:
  # Secret names are always required.
  secretName: kubernetes-dashboard-certs ### Secret名字
  duration: 2160h # 90d
  renewBefore: 360h # 15d
  subject:
    organizations:
    - Example Inc. ###
  # The use of the common name field has been deprecated since 2000 and is
  # discouraged from being used.
  commonName: my.k8s.com ###
  isCA: false
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048
  #usages: ### 注释了usages，使用情况是证书要求的x509使用情况的集合。默认为digital signature，key encipherment如果未指定。
  #  - server auth
  #  - client auth
  # At least one of a DNS Name, URI, or IP address is required.
  dnsNames:
  - my.k8s.com ###
  #uris: ### 注释了uris、ipAddresses
  #- spiffe://cluster.local/ns/sandbox/sa/example
  #ipAddresses:
  #- 192.168.0.5
  # Issuer references are always required.
  issuerRef:
    name: ca-issuer ### 使用CA Issuer
    # We can reference ClusterIssuers by changing the kind here.
    # The default value is Issuer (i.e. a locally namespaced Issuer)
    kind: ClusterIssuer ### CA Issuer是ClusterIssuer
    # This is optional since cert-manager will default to this value however
    # if you are using an external issuer, change this to that issuer group.
    group: cert-manager.io
```


### 将网站证书配置到Ingress
```shell
# site-example-com.ingress.example-com.yaml
# 参考：https://kubernetes.io/zh/docs/concepts/services-networking/ingress/#tls
kind: Ingress
apiVersion: extensions/v1beta1
metadata:
  name: site-example-com
  namespace: example-com
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  tls:
    - hosts:
        - my.k8s.com
      secretName: kubernetes-dashboard-certs
  rules:
    - host: my.k8s.com
      http:
        paths:
          - path: /
            pathType: ImplementationSpecific
            backend:
              serviceName: nginx
              servicePort: 80
```
