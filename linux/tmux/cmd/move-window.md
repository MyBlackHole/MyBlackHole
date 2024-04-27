# move-window 

```shell
-a -- 将窗口移动到目标窗口之后的下一个索引
-b -- 将窗口移动到目标窗口之前的下一个索引
-d -- 不要使新窗口成为活动窗口
-k -- 杀死目标窗口（如果存在）
-r -- 按顺序对会话中的窗口重新编号
-s -- 指定源窗口
-t -- 指定目标窗口
```

- 移动会话窗口到指定会话窗口后
```shell
tmux move-window -scode:1 -tcode:3 -a
```

- 重新编排序号
```shell
tmux move-window -r
```
