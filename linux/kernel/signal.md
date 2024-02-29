# signal

## SIGSEGV
SIG 是信号名的通用前缀， SEGV 是segmentation violation，也就是存储器区段错误。
1. 访问空指针
2. 内存越界访问
3. 访问已经释放的内存

- 如何避免SIGSEGV
申请内存之后，需要check 内存申请是否成功，然后再去访问内存。
确保申请的内存大小能满足使用的需求，避免越界访问。

![[imgs/Pasted image 20240229092229.png]]
![[imgs/Pasted image 20240229092345.png]]
## signal_struct

![[imgs/Pasted image 20240229092354.png]]

## sighand_struct

![[imgs/Pasted image 20240229092406.png]]
![[imgs/Pasted image 20240229092421.png]]

## sigpending和sigqueue

![[imgs/Pasted image 20240229092437.png]]
![[imgs/Pasted image 20240229092443.png]]


