# service

```shell
<!--server-->
xmake service --start

<!--client-->
cp service:/root/.xmake/service/client.conf ~/.xmake/service/client.conf
<!--连接同步文件-->
xmake service --connect
<!--同步文件-->
xmake service --sync

<!--拉去远程文件到本地-->
xmake service --pull 'build/**' outputdir

<!--断开连接-->
xmake service --disconnect

<!--查看服务器日志-->
xmake service --logs

<!--清理服务端缓存-->
xmake service --clean
```
