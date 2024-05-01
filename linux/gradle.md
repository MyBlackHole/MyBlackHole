# gradle

## install
```shell
wget https://objects.githubusercontent.com/github-production-release-asset-2e65be/696192900/30b914b2-256d-49b6-a68b-acaee9259642?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240205%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240205T022619Z&X-Amz-Expires=300&X-Amz-Signature=c9f60548777c2e9358cf50896398e2ba54ab4f9d6ed26ecb5e779c57b0ec6186&X-Amz-SignedHeaders=host&actor_id=39358316&key_id=0&repo_id=696192900&response-content-disposition=attachment%3B%20filename%3Dgradle-8.6-bin.zip&response-content-type=application%2Foctet-stream
mkdir /opt/gradle
unzip -d /opt/gradle gradle-8.6-bin.zip
ls /opt/gradle/gradle-8.6
```


## config

- 设置代理
```shell
lvim ~/.gradle/gradle.properties
# proxy settings
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=1080
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=1080

# or
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

## usage

1. build
```shell
gradle build
```
2. clean
```shell
gradle clean
```

3. run
```shell
gradle run
```

4. test
```shell
gradle test
```

5. dependency
```shell
gradle dependencies
```

6. wrapper
```shell
gradle wrapper
```

7. publish
```shell
gradle publish
```

8. install
```shell
gradle install
```
