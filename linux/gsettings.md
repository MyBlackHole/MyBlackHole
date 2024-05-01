# gsettings


```shell
# 通过命令行更改系统代理设置为手动
gsettings set org.gnome.system.proxy.http host 'my.proxy.com' 

gsettings set org.gnome.system.proxy.http port 8000 

gsettings set org.gnome.system.proxy mode 'manual' 

gsettings set org.gnome.system.proxy.https host 'my.proxy.com' 

gsettings set org.gnome.system.proxy.https port 8000 

gsettings set org.gnome.system.proxy.ftp host 'my.proxy.com' 

gsettings set org.gnome.system.proxy.ftp port 8000 

# 在命令行中更改系统代理设置为自动 

gsettings set org.gnome.system.proxy mode 'auto' 

gsettings set org.gnome.system.proxy autoconfig-url 
# 在命令行中清除系统代理设置 

gsettings set org.gnome.system.proxy mode 'none'
```

