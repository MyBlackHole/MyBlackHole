# opengauss

## build
[细节](https://zhuanlan.zhihu.com/p/364397999)
```shell
export CMAKEROOT=$cmake_prefix     ##编译cmake指定的--prefix
export GCC_PATH=$gcc_prefix        ##编译gcc指定的--prefix
export CC=$GCC_PATH/gcc/bin/gcc
export CXX=$GCC_PATH/gcc/bin/g++
export LD_LIBRARY_PATH=$GCC_PATH/gcc/lib64:$GCC_PATH/isl/lib:$GCC_PATH/mpc/lib/:$GCC_PATH/mpfr/lib/:$GCC_PATH/gmp/lib/:$CMAKEROOT/lib:$LD_LIBRARY_PATH
export PATH=$GCC_PATH/gcc/bin:$CMAKEROOT/bin:$PATH

# 修改 openGauss-third_party/build/get_PlatForm_str.sh 增加新的平台

if [ "$kernel"x = "ubuntu"x ]
then
    plat_form_str=ubuntu_"$cpu_bit"
fi
# $kernel 通过 lsb_release -d | awk -F ' ' '{print $2}'| tr A-Z a-z 获得
```

## docker
```shell
docker run --name opengauss --privileged=true -d -e GS_PASSWORD=openGauss@123 -p5432:5432 opengauss/opengauss:5.0.0

docker exec -it opengauss bash
su - opengauss
# 安装目录
cd /usr/local/opengauss

# 添加 opengauss lib 位置 (默认找不到)
sudo vim /etc/ld.so.conf.d/opengauss-x86_64.conf
/usr/local/opengauss/lib
# 使生效
ldconfig

# 连接测试
./gsql -d postgres -U gaussdb -W 'openGauss@123' -h 127.0.0.1 -p 5432

# docker client
docker run --rm --link=opengauss -p 8081:8081 opengauss/opengauss-webclient:1.0.4
```

## 命令
- 查询所有数据库
```shell
\l
```

- 查询数据目录
```shell
omm=# show data_directory;
     data_directory
-------------------------
 /var/lib/opengauss/data
(1 row)
```
