# 异常处理

- windows server12 r2 安装 python 失败
```
https://www.microsoft.com/en-us/download/details.aspx?familyid=373b1bb0-6d55-462e-98b7-6cb7d9ef1448 
自测只需要KB2919355就行 
（需更新3个补丁KB2919442 ，KB2919355，KB3118401  
其中先按装KB2919442 ，在KB2919355，最后KB3118401 
windows server 2012 x64过来反馈，更新windows update中所有可用更新后重启，顺序安装KB2887595、KB2919442、KB2919355、kB2999226，最后重启之后问题解决。） 
```

- pip3 install 安装报错： Missing dependencies for SOCKS support1 
解决：
```shell
# libqtshadowsocks-dev可能不需要 
sudo apt-get install qt5-qmake qtbase5-dev libqrencode-dev libqtshadowsocks-dev libappindicator-dev libzbar-devlibbotan1.10-dev
```

- gcc收到一个错误的退出状态
未安装软件
安装对应软件

- pip install 出现 SSL
源或网络有问题

- pip 网络超时
-i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com 
-i https://mirrors.163.com/pypi/simple 

- Pip2 与 pip3共存
python脚本python指向问题

- ssl 编译
```shell
# 拉取 Openssl-1.1.1
echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/openssl/lib" >> $HOME/.bash_profile 
source $HOME/.bash_profile 
cd Openssl-1.1.1
./configure
make
```

- pycharm: Error:Can't get remote credentials for deployment server root@172.17.0.2:22 
![[imgs/GetImage.png]]

- json字符串使用的是“”

- 编码与解码
str-> encode（编码） ->bytes 
bytes->decode（解码）->str 

- 安装opencc包后出现错误：OSError: dlopen(libopencc.so.1, 6): image not found
```shell
pip install opencc 
# 改成 
pip install opencc-python-reimplemented==0.1.4 
```

- UnicodeEncodeError: 'gbk' codec can't encode character '\xbb' in position 8530: illegal multibyte sequence 
```python
# python的默认编码不是'utf-8',改一下python的默认编码成'utf-8'就行了 
import io 

import sys 

#改变标准输出的默认编码 

sys.stdout = io.TextIOWrapper(sys.stdout.buffer,encoding='utf8') 
```

- python flask报错ImportError: cannot import name 'cached_property' from 'werkzeug'
```python
from werkzeug import cached_property 
# 改成 
from werkzeug.utils import cached_property 
```

- python.exe 应用程序错误 内存不能read written 
```ps
# 运行注册所有dll 
for %1 in (%windir%\system32\*.dll) do regsvr32.exe /s %1 
```

- unable to get local issuer certificate (_ssl.c:1045) 
```python
# 全局取消证书验证 
import ssl 
ssl._create_default_https_context = ssl._create_unverified_context 
```

- Non-ASCII character '\xe2' in file 
```python
# 建议在文件头追加： 

# -*- coding: cp936 -*- 
## 或者 
# -*- coding: utf-8 -* 

```

-  Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": https://visualstudio.microsoft.com/downloads/ 
VS 
win10 sdk 
c++构建工具 

-  cannot import name 'cached_property' from 'werkzeug' 
```shell
# flask问题 
pip install werkzeug==0.16.0 
```

- 由于系统缓冲区空间不足或队列已满,不能执行套接字上的操作 
```
regedit 
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\MaxUserPort如果没有，则手动创建 DWord（32位）  ”数值数据“改为十进制65534或者认为适当的值。 
此值表示 用户最大使用的端口数量，默认为5000。（设置好这个就行了，下面酌情处理） 

HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\TCPTimedWaitDelay 如果没有，则手动创建 DWord（32位）  ”数值数据“改为十进制30 或者你认为适当的值。 
此值表示一个关闭后的端口等待多久之后可以重新使用，默认为120秒，也就是2分钟才可以重新使用。 
```

- python 没有pip
```shell
python -m ensurepip
```

- ubuntu14 安装python3.7 
```shell
# 自带libssl-dev不支持 需要手动编译安装 OpenSSL 
wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz 
tar zxvf openssl-1.1.1g.tar.gz 
cd openssl-1.1.1g 
./config --prefix=/home/username/openssl --openssldir=/home/username/openssl no-ssl2 
make 
make test 
make install 
export PATH=$HOME/openssl/bin:$PATH 
export LD_LIBRARY_PATH=$HOME/openssl/lib 
export LC_ALL="en_US.UTF-8" 
export LDFLAGS="-L/home/username/openssl/lib -Wl,-rpath,/home/username/openssl/lib" 
```

- 'utf-8' codec can't encode characters in position 267-268: surrogates not allowed 
UnicodeEncodeError: 'utf-8' codec can't encode characters in position 18330-18331: surrogates not allowed 

```python
import json 
ll = {'caption': '#熊二来电 狗熊来电\ud83d\ude4b @趙雲哥哥\ud83d\udd31(风华绝代浪浪) @《龍七团》姜少❗️(风华浪浪)'} 
tt = json.dumps(ll) 转成字符串 
dd = json.loads(tt)  再转成dicts 
print(dd['caption']) 
print(dd.get('caption')) 
```

- required field "type_ignores" missing from Module 
951行加上   ,[]  
修改成
module = ast.fix_missing_locations(ast.Module([func_ast], [])) 

- RuntimeError: cryptography is required for sha256_password or caching_sha2_password 
```shell
pip install cryptography 
```

- ImportError: No module named 'yaml' 
```shell
pip3 install pyyaml 
```
