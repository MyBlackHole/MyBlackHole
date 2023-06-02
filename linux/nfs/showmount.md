# showmount 

## 安装
```shell
sudo apt install nfs-common
```

## 参数
```
-a|--all: 以 host:dir 这样的格式来显示客户主机名和挂载点目录。 
-d|--directories: 仅显示被客户挂载的目录名 
-e|--exports: 显示NFS服务器的输出清单 
-h|--help: 显示帮助信息 
-v|--version: 显示版本信 
--no-headers: 禁止输出描述头部信息 
```

## 使用
- 查看指定 ip NFS服务器的全部共享目录
```shell
showmount -e IP
```

- 显示指定 nfs 服务器的客户端信息和共享目录
```shell
showmount -a IP
```

- 获取已经被客户端加载的NFS共享目录
```shell
showmount -d IP
```

- 显示指定 NFS 服务器连接 nfs 客户端的信息
```shell
showmount IP
```
