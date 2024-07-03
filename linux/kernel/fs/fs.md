# fs

- 推荐实现 demo
```shell
<!-- 初级 -->
git@github.com:sysprog21/simplefs.git
git@github.com:psankar/simplefs.git

<!-- 中级 -->

<!-- 高级 -->
git@github.com:torvalds/linux.git
bcachefs
```

- 查看系统最大文件句柄数
```shell
<!-- nofile：1024*1024（2.6.25内核之前）。如果是2.6.25之后的内核，使用该命令查询 -->
cat /proc/sys/fs/nr_open

```
