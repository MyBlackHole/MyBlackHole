# GTM

Gtm的作用一句话概括就是：
为了保证数据的全局读一致性。
这里有个误区，可能有人认为如果没有gtm就会造成节点间数据不一致，这种说法是错误的
gtm 是为了保证某一时刻读到一致的数据
而**写一致性**是通过**两阶段提交**保证的

![[imgs/GTM.png]]
