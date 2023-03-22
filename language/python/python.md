# python

## 安装单版本
```shell
# 下载源码
wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz 
# 创建安装目录
mkdir –p /usr/loacl/python3.6/ 
# 解压
tar zxvf Python-3.6.1.tgz 
# 编译安装
./configure --prefix=/usr/local/python3.6 
make 
make install 
sudo ln –s /usr/local/python3.6/bin/python3.6 /usr/bin/python3.6 
/usr/local/python3.6/bin添加入PATH 
export PATH=/usr/local/python3.6/bin:$PATH 
```

## 安装多版本
- pyenv (版本控制)
```shell
# 安装 pyenv
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \ 
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \ 
xz-utils tk-dev libffi-dev liblzma-dev 
sudo apt install git curl
curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash 

# 查看当前版本
pyenv version

# 查看所有版本
pyenv versions

# 删除指定版本
pyenv uninstall 3.5.2

# 查看所有可安装的版本
pyenv install --list 

# 指定全局版本 
pyenv global 3.6.5 

# 指定多个全局版本, 3版本优先
pyenv global 3.6.5 2.7.14 

# 添加环境配置 
eval "$(pyenv init --path)" 
eval "$(pyenv virtualenv-init -)" 
```

- pyenv-virtualenv
```shell
# 创建进入项目环境 
pyenv virtualenv 3.8.1 first_project 

# 切换项目环境
pyenv activate first_project 

# 退出当前环境 
pyenv deactivate 

# 删除项目环境 
pyenv virtualenv-delete first_project 
```

- 源码编译
```shell
# ubuntu
apt install libffi-dev 
# centos
Yum install libffi-devel 
./configur  
# 自定义导入 
make Programs/_freeze_importlib 
./Programs/_freeze_importlib importlib_bootstrap Lib/importlib/_bootstrap.py Python/importlib.h 
./Programs/_freeze_importlib importlib_bootstrap_external Lib/importlib/_bootstrap_external.py Python/importlib_external.h 
make 
make install  
```

# 配置
## 更改源
临时使用 -i  https://pypi.douban.com/simple/ 
```
cd 
mkdir .pip 
nvim pip.conf 
#在.pip目录创建并编辑pip.conf 
#pip安装需要使用的https加密，所以在此需要添加trusted-host 

cat > pip.conf << EOF 
[global] 
trusted-host = mirrors.aliyun.com 
index-url = https://mirrors.aliyun.com/pypi/simple/  
EOF 
```

## windows
```
a、在cmd窗口下执行echo %HOMEPATH%获取用户家目录，并在该目录下创建pip目录。 
b、在pip目录下创建pip.ini文件。记住，后缀必须是.ini格式。并在该文件中写入如下内容。 

[global] 
index-url = http://pypi.douban.com/simple 
[install] 
trusted-host = pypi.douban.com 
```

## 导包
```shell
pip freeze > requirements.txt 
```

## 安装包
```shell
pip install -r requirements.txt 
```

## 打包
```shell
# 打出的桌面程序去掉命令行黑框 
pyinstaller.exe -F xxx.py -w 

-w:打包exe去掉cmd窗口 
-F:打包成单个exe 
-D:打包成一个文件夹 
-i:设置默认图标 
```


# 命令案例
-  更新pip
```shell
python -m pip install --upgrade pip
```

- 包列表
```shell
pip list # 弃用了

pip freeze 
```
