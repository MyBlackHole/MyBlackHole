# rocksdb
基于 leveldb 的 key-alue 数据库

## 安装 lib
```shell
apt-get install build-essential libgflags-dev libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev
git clone https://github.com/facebook/rocksdb.git
cd rocksdb
mkdir build
cd build
make shared_lib
sudo make install-shared INSTALL_PATH=/usr
```

## 例子


