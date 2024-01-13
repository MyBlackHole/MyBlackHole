# gradle

- 设置代理
```shell
org.gradle.jvmargs=-DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1080
org.gradle.jvmargs=-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=1080 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=1080

---

gradle -Dhttp.proxyHost=www.somehost.org -Dhttp.proxyPort=8080 -Dhttp.proxyUser=userid -Dhttp.proxyPassword=password -Dhttp.nonProxyHosts=*.nonproxyrepos.com|localhost

---

<!-- gradle.properties -->
<!-- 此处配置代理的域名或者IP -->
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=1080
<!-- proxyUser和proxyPassword如果没有可以不配置 -->
<!-- systemProp.https.proxyUser=userid -->
<!-- systemProp.https.proxyPassword=password -->
<!-- 此处是不使用代理的host列表，可以把内网地址和国内的地址添加上去。 -->
<!-- systemProp.https.nonProxyHosts=*.nonproxyrepos.com|localhost -->
```
