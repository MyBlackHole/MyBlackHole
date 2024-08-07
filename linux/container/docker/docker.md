# docker (推荐 podman)
## install

### 离线安装
```shell
# 内网下载 
wget http://10.7.102.125:8000/downloads/docker-18.06.3-ce.tgz 
tar -zxvf docker-18.06.3-ce.tgz
cp docker/* /usr/bin/

cat << EOF > /etc/systemd/system/docker.service  
[Unit] 
Description=Docker Application Container Engine 
Documentation=https://docs.docker.com 
After=network-online.target firewalld.service 
Wants=network-online.target 

[Service] 
Type=notify 
# the default is not to use systemd for cgroups because the delegate issues still 
# exists and systemd currently does not support the cgroup feature set required 
# for containers run by docker 
ExecStart=/usr/bin/dockerd --selinux-enabled=false 
ExecReload=/bin/kill -s HUP $MAINPID 
# Having non-zero Limit*s causes performance problems due to accounting overhead 
# in the kernel. We recommend using cgroups to do container-local accounting. 
LimitNOFILE=infinity 
LimitNPROC=infinity 
LimitCORE=infinity 
# Uncomment TasksMax if your systemd version supports it. 
# Only systemd 226 and above support this version. 
#TasksMax=infinity 
TimeoutStartSec=0 
# set delegate yes so that systemd does not reset the cgroups of docker containers 
Delegate=yes 
# kill only the docker process, not all processes in the cgroup 
KillMode=process 
# restart the docker process if it exits prematurely 
Restart=on-failure 
StartLimitBurst=3 
StartLimitInterval=60s 

[Install] 
WantedBy=multi-user.target 
EOF 

chmod +x /etc/systemd/system/docker.service  
systemctl daemon-reload  
```

### 在线安装
- centos
```shell
yum install -y epel-release
yum install -y yum-utils device-mapper-persistent-data lvm2 

cd /etc/yum.repos.d
wget http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

yum list docker-ce --showduplicates | sort -r 

yum install docker-ce docker-ce-cli containerd.io

systemctl start docker 
systemctl enable docker 
```

- ubuntu
```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun (一键) 
sudo add-apt-repository \ 
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \ 
   $(lsb_release -cs) \ 
   stable" 
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 7EA0A9C3F273FCD8 
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common 
sudo add-apt-repository \ 
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \ 
    $(lsb_release -cs) \ 
    stable" 

sudo apt-get install docker-ce 
```

- win linux 子系统
```shell
先关闭原来的管理员WSL界面，重新开启一个 
首先需要进入管理员的WSL，然后直接进入root用户，直接在root用户启动docker，就可以了 
service docker start 
```

- win
```shell
安装 https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi  
安装 Docker Desktop Installer 
```

## remove
```shell
# 官方给的删除方案 
sudo apt remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-ce 
```

### 参数

- --link
```shell
--link可以通过容器名互相通信，容器间共享环境变量。
--link主要用来解决两个容器通过ip地址连接时容器ip地址会变的问题.
```


## 配置

- 远程访问
```shell
# Ubuntu文件路径
/etc/default/docker  
/lib/systemd/system/docker.service 

# 修改/etc/sysconfig/docker文件，在最后增加一行DOCKER_OPTS 
DOCKER_OPTS="-H unix:///var/run/docker.sock -H 0.0.0.0:5555" 

# 修改/usr/lib/systemd/system/docker.service 
在[Service]的ExexStart=下面增加一行$DOCKER_OPTS; 
也可以直接在ExecStart后面追加-H unix:///var/run/docker.sock -H 0.0.0.0:5555 

systemctl daemon-reload(重新加载守护进程) 
systemctl restart docker.service  

查看端口是否开放：ss -l 
测试：curl IP:POST/info 
```

- 配置 DNS
```shell
lvim /etc/docker/daemon.json  
{ "dns" : [ "114.114.114.114", "8.8.8.8" ] }  

sudo service docker restart 
```

- 配置私有仓库地址 
```shell
vim /etc/docker/daemon.json 
{
    "insecure-registries": ["192.168.5.229:8089"] 
}
```

- 镜像加速
```shell
vim /etc/default/docker 
添加：DOCKER_OPTS=“--registry-mirror=URL”  

# 或
sudo mkdir -p /etc/docker 
sudo tee /etc/docker/daemon.json <<-'EOF' 
{ 
  "registry-mirrors": ["https://98bnyivl.mirror.aliyuncs.com"] 
} 
EOF 


sudo systemctl daemon-reload 
sudo systemctl restart docker 
```

- 用户配置
```shell
# 添加www用户并修改密码 
useradd www 

# 修改www账号密码 
passwd www 

# 配置 sudo 执行权限
echo 'www     ALL=(ALL)NOPASSWD:ALL' >> /etc/sudoers 
sed -i 's/Defaults    requiretty/#Defaults    requiretty/g' /etc/sudoers 

# 添加docker用户组 
groupadd docker 

# 把需要执行的docker用户添加进该组，这里是www用户 
gpasswd -a www docker 

# 重启 docker 
systemctl restart docker 

# 测试,运行
su www 
docker ps -a  
```

- 日志限制,只对新建容器有效
```shell
"log-opts": {"max-size":"500m", "max-file":"3"} 
```

## 启动
```shell
#启动 
systemctl start docker 

#设置开机启动 
systemctl enable docker.service 

#查看docker服务状态 
systemctl status docker 
```

## 例子

- 镜像存放位置
默认情况下Docker的存放位置为:/var/lib/docker 

- 删除所有关闭的容器
```shell
docker ps -a | grep Exit | cut -d ’ ’ -f 1 | xargs docker rm
```

- 删除关闭的容器、无用的数据卷和网络
```shell
docker system prune
```

- 删除更彻底，可以将没有容器使用Docker镜像都删掉
```shell
docker system prune -a
```

- 设置 docker 打开文件数量
```shell
<!-- docker 执行 run 加上 -->
--ulimit nofile=65535:65535
```
