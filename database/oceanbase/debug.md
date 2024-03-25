# debug

```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/oceanbase/OceanBase.repo
sudo yum install -y libtool libaio obclient
cd {your_oceanbase_project}/tools/deploy # 进入项目目录下的tools/deploy目录
./obd.sh prepare # 准备配置文件
./obd.sh deploy -c single.yaml # 根据配置文件，在本地部署一个ObServer


cd {your_oceanbase_project}/tools/deploy 
./obd.sh gdb

<!-- 二次启动可以 -->
bin/observer

gdb bin/observer

```

```shell
/media/black/Data/Documents/github/Cpp/oceanbase/deps/3rd/u01/obclient/bin/obclient -h 127.0.0.1 -P 10000 -uroot
```


```shell
apt-get install git wget rpm rpm2cpio cpio make build-essential binutils
bash build.sh debug --init --make
```

- 归档逻辑
```shell
ob_archive_sender.cpp:ObArchiveSender::handle
```
