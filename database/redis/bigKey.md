# bigkey

## 描述
大key问题是某个key的value比较大
所以本质上是大value问题
key往往是程序可以自行设置的
value往往不受程序控制
因此可能导致value很大

例如
- redis的key是歌单ID,redis的value是个list,list包含了用户ID,用户可能很多,就导致list长度不可控
![[imgs/GetImage.jpeg]]

## 影响
redis核心工作是单线程的(新版中多线程只是用来处理请求任务)
单线程中命令任务的处理是串行

## 找大Key
- bigkeys
