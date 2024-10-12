# 

- WARNING: Your password has expired. Password change required but no TTY available.
[[chage]]
```shell
<!-- 密码过期 -->
设置永远
chage -M 99999 euser
```
- /usr/bin/ssh-copy-id: ERROR: failed to open ID file '/root/.pub': 没有那个文件或目录
        (to install the contents of '/root/.pub' anyway, look at the -f option)
```shell
ssh-keygen -t rsa -C "1358244533@qq.com"
```

- Privilege separation user sshd does not exist
```shell

nvim /etc/passwd
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin

nvim /etc/group
sshd:x:74:

<!--Missing privilege separation directory: /var/empty-->
mkdir /var/empty


---或---

SSHD 配置中禁用 UsePrivilegeSeparation。编辑文件 /etc/ssh/sshd_config 并更改

UsePrivilegeSeparation yes
to
UsePrivilegeSeparation no

它不太安全，但只是另一种选择。


<!--/etc/init.d/sshd restart-->
```
