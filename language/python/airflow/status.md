# airflow status

在airflow中，通过对pipeline中的不同task赋予不同的状态（state）说明当前任务执行进度。通过airflow的状态机制我们可以很好的把握当前任务的执行进度。

airflow2.0.1相较airflow1.9.0，对任务状态进行了进一步的细化。本文中，主要是为了对airflow的基础状态进行介绍和记录。

## 1. airflow状态的作用

ariflow中的状态标识类似红绿灯，分成了红、黄、绿三种基础颜色。在该基础上有对三种不同的颜色进行了进一步的细化来标识不同的细化状态。



在airflow2.0.1中，airflow状态主要分成两大类：

1.  Dagrun：SUCCESS, RUNNING, FAILED
2.  Task：SUCCESS，RUNNING，FAILED, UPSTREAN_FAILED, SKIPPED, UP_FOR_RETRY, UP_FOR_RESCHEDULE, QUEUED, NONE, SCHEDULED

Dagrun状态是对应dag的主状态，当该状态为FAILED的时候，即使有task依旧是running，airflow也会对这些task进行清楚。

Task状态是对Dagrun的细化状态。因为在Dag中不同task之间可能会有互相之间的依赖。

针对airflow状态的另外分类方法是：完成（ **finished** ）和未完成（ **unfinished** ）两个大类。

+   完成状态代表airflow scheduler不会在对当前task/dagrun进行监控
+   未完成状态代表当前task/dagrun的状态还会发生变更，最终会达到某一个完成状态。

## 2. airflow状态的详细介绍

### 2.1 SUCCESS（dagrun | task | finished）



+   success状态说明当前任务在执行过程中没有遇到任何错误并且正常完成。
+   airflow scheduler上报状态之后释放针对当前任务的监控
+   executor执行完成对应任务
+   颜色：墨绿色

### 2.2 RUNNING（dagrun | task | unfinished）



+   running状态代表当前任务正在执行中
+   airflow scheduler将该任务提交到了executor上，并对当前任务通过heartbeat进行监控
+   executor正在对对应的实际任务进行执行
+   颜色：浅绿色
+   后续状态：SUCCESS、RETRY、FAILED

### 2.3 FAILED（dagrun | task | finished）



+   failed状态说明当前任务执行过程中出现问题，返回了错误代码。
+   airflow scheduler会上报当前任务failed状态，和下游任务的UPSTREAM_FAILED状态
+   executor执行完对应任务
+   颜色：红色

### 2.4 UPSTREAM_FAILED（task | finished）



+   upstream_failed 状态说明当前任务的上游任务发生错误，处于FAILED状态，当前任务并没有开始执行
+   airflow scheduler 直接上报当前任务的状态，并不会将当前任务进行调度
+   executor 没有执行对应任务
+   颜色：棕黄色

### 2.5 SKIPPED（task | finished）



+   skipped 状态代表当前任务被跳过，没有执行，经常发生在branchOperator未触发的分支上
+   airflow scheduler 绕过了当前任务，没有进行调度
+   executor 没有执行对应任务
+   颜色：浅红色

### 2.6 UP_FOR_RETRY（task | unfinished）



+   up_for_retry 代表当前任务执行失败并准备重试
+   airflow scheduler 会在retry interval 之后，下一次heartbeat到达时重新调度该任务
+   executor 上次执行对应任务失败，当前没有执行对应任务
+   颜色：黄色

### 2.7 UP_FOR_RESCHEDULE（task | unfinished）



+   up_for_reschedule 实在airflow1.10.2中引入的新状态，常用在Sensor中，可以避免消费slots
+   airflow scheduler 会对当前任务进行定时重试，防止长时间占据woker slots，从而导致worker锁死，无法执行其他任务
+   executor 定时执行对应任务
+   颜色：浅绿色

### 2.8 QUEUED（task | unfinished）



+   queued 状态代表当前任务已经被调度，但是正在等待一个可用的executor slots.,
+   airflow scheduler 已经将该任务调度，正在排队等待可用slots
+   executor 没有空闲slots执行该任务
+   颜色：灰色

### 2.9 NO_STATUS（task | unfinished）



+   NO_STATUS代表当前任务还没有被调度到，前面任务正在执行
+   颜色：无色

### 2.10 SCHEDULED（task | unfinished）



+   scheduled 状态代表该任务已经被触发
+   airflow scheduler 调度该任务，但是并没有open slots ( = all_slots - running_slots - queued_slots)，正在轮询状态中
+   executor没有执行任务
+   颜色：棕色
