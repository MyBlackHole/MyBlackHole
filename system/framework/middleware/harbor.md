# harbor

```shell
helm repo add harbor https://helm.goharbor.io
helm fetch harbor/harbor --untar

helm repo add harbor https://helm.goharbor.io
helm install harbor-bh harbor/harbor

helm install -f ./harbor/values.yaml harbor ./harbor --namespace harbor --create-namespace
```
