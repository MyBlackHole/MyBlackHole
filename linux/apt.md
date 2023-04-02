# ubuntu 包管理器

## 参数
- install -y
安装指定的软件包

- autoclean
清除旧版
自动清除在安装过程中下载的deb安装包 

- update
更新源里面的索引列表 /etc/apt/sources.list 源的配置
更新下来的东西放在 /var/lib/apt 目录下

- dist-upgrade
此会强制更新软件包到最新版本，并自动解决修复依赖关系。 

- remove
卸载指定的软件包 

- autoremove
自动删除无用的安装包 

- purge
卸载指定的软件包，并把配置文件也删除掉 

- upgrade 
更新已经安装的软件包 

- clean
清除在安装过程中下载的deb安装包 
apt的缓存位置在/var/cache/apt/目录下。 
安装过程中下载的deb安装包缓存在/var/cache/apt/archives目录下 

- source
下载指定安装包的源代码到当前目录下 

- download
下载指定的deb安装包到当前目录下 

- changelog
查看指定deb安装包的更新日志列表 

- check 
检测指定deb安装包的依赖关系是否被破坏了 

## 例子
- 使用代理请求
```shell
sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:1080" install ripgrep 
```
