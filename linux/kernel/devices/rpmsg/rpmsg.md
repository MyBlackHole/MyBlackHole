# rpmsg

支持数据长度：512Bytes（496Bytes数据有效载荷，前16Bytes称为rpmsg标头，由传输层内部使用，其中包含源地址、目标地址和数据大小。）
传输方式：使用mbox作为中断传递，在mbox中传递vqid（Virtual Queue ID，标识虚拟消息队列的id），实际的IPC数据存在rpmsg-buf区的VRing中。
应用场景：较长控制命令

![[imgs/Pasted image 20240202233509.png]]

![[imgs/Pasted image 20240202233454.png]]

