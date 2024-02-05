# error 
- error   ingress/ingress.go:472  failed to get APISIX gateway external IPs       {"error": "resource name may not be empty"}
[issues](https://github.com/apache/apisix-ingress-controller/issues/1880)
```shell
values.yaml
  # -- the controller will use the Endpoint of this Service to
  # update the status information of the Ingress resource.
  # The format is "namespace/svc-name" to solve the situation that
  # the data plane and the controller are not deployed in the same namespace.
  ingressPublishService: "ingress-apisix/apisix-gateway"
```
