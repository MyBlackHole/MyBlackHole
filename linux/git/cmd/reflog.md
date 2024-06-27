# reflog

- 查看历史
```shell
git reflog

72e446ac0 (HEAD -> file_ob_check_log) HEAD@{0}: reset: moving to 72e446ac0
16410a611 (origin/file_ob_check_log) HEAD@{1}: checkout: moving from 4.3.0.2-release to file_ob_check_log
c3d2d1c3a HEAD@{27}: commit: 【3016925601】支持 ob 日志文件连续性检测
```

- 回退到指定版本(恢复到某次提交)
[推荐连接](https://blog.csdn.net/logan_LG/article/details/81531796)
```shell
git reset --hard c3d2d1c3a
```
