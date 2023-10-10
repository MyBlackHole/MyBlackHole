# proxyro 

obclient -h127.1 -uroot@sys -P2881 -p -c -A

- 创建
```shell
CREATE USER proxyro IDENTIFIED BY 'xxxxxxxxxx';
```


- 修改密码
```shell
ALTER USER proxyro IDENTIFIED BY password;

SET PASSWORD FOR proxyro = password('wwDD99__ob');
SET PASSWORD FOR proxyro = password('aaAA11__ob');
```


```shell
/opt/taobao/install/obproxy-3.3.1/bin/obproxy -p 2883 -n ob110145146147_proxy
```


```shell
<!-- ob集群信息查询 -->
select * from __all_server;
```
