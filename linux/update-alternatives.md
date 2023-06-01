# update-alternatives
进行版本切换
![[imgs/Pasted image 20230428022352.png]]

- 修改 gcc
```shell
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 100
sudo update-alternatives --config gcc
```

- 修改默认浏览器
```shell
sudo update-alternatives --config x-www-browser
```
