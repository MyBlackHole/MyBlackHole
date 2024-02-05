# consul

```shell
docker run -d -p 8500:8500 --restart=always --name=consul consul:1.15.4 agent -server -bootstrap -ui -node=1 -client='0.0.0.0'
```
