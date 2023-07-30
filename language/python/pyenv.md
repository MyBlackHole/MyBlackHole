# pyenv


## 安装
```shell
sudo add-apt-repository ppa:deadsnakes/ppa
# 安装 pyenv
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
    libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
    xz-utils tk-dev libffi-dev liblzma-dev
sudo apt install git curl
curl https://pyenv.run | bash

# 添加环境配置 
# Python
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

eval "$(pyenv virtualenv-init -)"

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

# 指定版本安装 
pyenv install 3.6.8 (下载慢可以先下载放到 ~/.pyenv/cache)

# 出现 segmentation 错误
CC=clang pyenv install 3.6.8

# 指定多个全局版本, 3版本优先
pyenv global 3.6.5 2.7.14 

```

- pyenv-virtualenv
```shell
# 创建进入项目环境 
pyenv virtualenv 3.8.1 first_project 
pip3.8
python3.8

# 切换项目环境
pyenv activate first_project 

# pip upgrade
pip3 install --upgrade pip

# 退出当前环境 
pyenv deactivate 

# 删除项目环境 
pyenv virtualenv-delete first_project 
```
