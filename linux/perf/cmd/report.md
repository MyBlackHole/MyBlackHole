# report

- 浏览数据
```shell
perf report

<!--self：self记录的是最后一列的符号（可以理解为函数）本身采样数占总采样数的百分比-->
<!--目的： 找到最底层的热点函数-->

<!--Children：记录的是这个符号调用的其他符号（理解为子函数，包括直接调用和间接调用）的采样数之和占总采样数的百分比-->
<!--目的：找到叫高层的热点函数-->

// h/?/F1 显示此窗口
// UP/DOWN/PGUP
// PGDN/SPACE 导航
// q/ESC/CTRL+C 退出浏览器或返回上一屏幕
// 
// 对于多个事件会话：
// 
// TAB/UNTAB 切换事件
// 
// 对于符号视图 (--sort has sym):
// 
// ENTER 放大 DSO/Threads 并注释当前符号
// ESC 缩小
// + 展开/折叠一个调用链级别
// a 注释当前符号
// C 折叠所有调用链
// d 放大当前 DSO
// e 展开/折叠主条目调用链
// E 展开所有调用链
// F 切换已过滤条目的百分比
// H 显示列标题
// k 放大内核映射
// L 更改百分比限制
// m 显示上下文菜单
// S 放大当前处理器插槽
// i 显示标题信息
// P 将直方图打印到 perf.hist.N
// r 运行可用脚本
// s 切换到另一个PWD 中的数据文件
// t 放大到当前线程
// V 详细（调用链中的 DSO 名称等）
// / 按名称过滤符号
// 0-9 按组中的事件 n 排序
```
