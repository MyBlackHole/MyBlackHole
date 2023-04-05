# top
![[imgs/Pasted image 20230405110419.png]]

细节[https://blog.csdn.net/u011391839/article/details/107407466]

## 系统状态
top - 11:07:38 up 28 min,  1 user,  load average: 0.54, 0.72, 0.81

分别对应：系统当前时间、系统到目前为止已运行的时间、当前登录系统的用户数量、系统负载（任务队列的平均长度），3个数值分别为1分钟、5分钟、15分钟前到现在的平均值

提示：
    top给出的系统运行时间，反应了当前系统存活多久，对于某些应用而言，系统需要保证7*24小时的高可用性，这个字段信息就能很好的衡量系统的高可用性
 
## 进程状态
任务: 431 total,   2 running, 429 sleeping,   0 stopped,   0 zombie
分别对应：所有启动的进程数、正在运行的进程数、挂起的进程数、停止的进程数、僵尸进程数


## CPU
%Cpu(s):  3.0 us,  1.7 sy,  0.0 ni, 95.0 id,  0.1 wa,  0.0 hi,  0.2 si,  0.0 st
分别对应：用户空间占用CPU百分比、内核空间占用CPU百分比、用户进程空间内改变过优先级的进程占用CPU百分比、空闲CPU百分比、等待输入输入的CPU百分比、硬中断占用CPU百分比、软中断CPU百分比、虚拟CPU等待实际CPU的时间的百分比

- 按下键盘 1, 显示各逻辑 cpu 资源使用情况
%Cpu0  :  0.7 us,  2.0 sy,  0.0 ni, 95.3 id,  0.0 wa,  0.0 hi,  2.0 si,  0.0 st
%Cpu1  :  0.8 us,  1.0 sy,  0.0 ni, 98.0 id,  0.3 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu2  :  1.8 us,  1.8 sy,  0.0 ni, 96.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu3  :  1.7 us,  2.2 sy,  0.0 ni, 96.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu4  :  5.2 us,  2.0 sy,  0.2 ni, 92.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu5  :  1.7 us,  0.7 sy,  0.0 ni, 97.0 id,  0.0 wa,  0.0 hi,  0.5 si,  0.0 st
%Cpu6  :  3.5 us,  1.8 sy,  0.0 ni, 94.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu7  :  0.3 us,  0.8 sy,  0.0 ni, 99.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu8  :  0.8 us,  2.0 sy,  0.0 ni, 97.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu9  :  1.0 us,  1.0 sy,  0.0 ni, 98.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu10 :  1.0 us,  2.0 sy,  0.0 ni, 97.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu11 :  1.0 us,  1.3 sy,  0.0 ni, 97.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu12 :  2.0 us,  1.8 sy,  0.0 ni, 96.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu13 :  0.8 us,  1.0 sy,  0.0 ni, 98.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu14 :  3.0 us,  2.3 sy,  0.0 ni, 94.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu15 :  0.8 us,  0.8 sy,  0.0 ni, 98.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
