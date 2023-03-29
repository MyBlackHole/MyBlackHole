#  

## 安装
```shell
mkdir /data
useradd -d /data/tbase tbase

yum install -y bison.x86_64 bison-devel.x86_64 flex.x86_64 flex-devel.x86_64
yum install -y readline readline-dev
yum install -y readline.x86_64 readline-devel.x86_64
yum install -y zlib.x86_64 zlib-devel.x86_64
yum install -y openssl-devel
yum install -y uuid uuid-devel
yum install -y git.x86_64

git clone https://github.com/Tencent/TBase.git

mkdir -p /data/tbase/install
chown -R tbase:tbase /data
export SOURCECODE_PATH=/data/tbase/TBase
export INSTALL_PATH=/data/tbase/install

cd ${SOURCECODE_PATH}
chmod +x configure*
./configure --prefix=${INSTALL_PATH}/tbase_bin_v2.0  --enable-user-switch --with-openssl  --with-ossp-uuid CFLAGS=-g

make clean
make -sj
make install

chmod +x cnotrib/pgxc_ctl/make_signature
cd contrib
make -sj
make install
```
