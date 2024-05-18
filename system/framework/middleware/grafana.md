# grafana

## install

- podman
```
podman run -d --name=grafana -p 3000:3000 grafana/grafana
<!-- 默认用户名密码 admin/admin -->

<!-- 网络模式 host 启动 -->
podman run -d --name=grafana --network=host grafana/grafana
```

## add data source
- loki
```
http://127.0.0.1:3000/connections/add-new-connection

Name: loki
Type: Loki
URL: http://loki:3100
```

