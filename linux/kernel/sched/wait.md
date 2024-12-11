# wait

等待队列, 等待特定条件触发的进程, 并将其移入相应的等待队列.

## wait 函数


### 普通 wait 函数

#### 不可中断，没有超时的wait函数

该方法在不满足条件condition时会进行等待，
一直等条件满足为止，期间没有时间限制，不允许中断

```c
#define wait_event(wq, condition) 	
do {	
	// 判断，如果条件为真则不用调用等待队列，继续执行
	if (condition)
		break;
    // 调用等待队列
	__wait_event(wq, condition); 
} while (0)

#define __wait_event(wq, condition>
do {
    // 定义一个等待线程
	DEFINE_WAIT(__wait);
	for (;;) {
		// 将当前进程挂到等待队列，并设置当前状态
		prepare_to_wait(&wq, &__wait, TASK_UNINTERRUPTIBLE);
		if (condition)
			break;
		// 进程调度，使等待队列开始休眠
		schedule();
	}
	//把唤醒的进程从等待队列中去掉，并设置当前状态为TASK_RUNNING
	finish_wait(&wq, &__wait);
} while (0)
```

#### 不可中断，有超时限制的wait函数

该方法在超时情况下未能得到满足的条件会返回0，
否则返回剩余的可等待时间，期间不可中断。

```c
#define wait_event_timeout(wq, condition, timeout)	
({							
	long __ret = timeout;	//设置最大等待时间			
	if (!(condition)) 	//如果条件不满足，调用等待队列			
		__wait_event_timeout(wq, condition, __ret);
	__ret;							
})

#define __wait_event_timeout(wq, condition, ret)
do {						
	DEFINE_WAIT(__wait);//定义一个等待线程
					
	for (;;) {	
		//将当前进程挂到等待队列，并设置当前状态
		prepare_to_wait(&wq, &__wait, TASK_UNINTERRUPTIBLE);
		if (condition)
			break;
		//进程调度，使等待队列开始休眠，且有最大休眠时间限制
		ret = schedule_timeout(ret);
		if (!ret)
			break;
	}
	//把唤醒的进程从等待队列中去掉，并设置当前状态为TASK_RUNNING
	finish_wait(&wq, &__wait);
} while (0)
```

#### 可中断，没有超时限制的wait函数

该方法在等待时如果被中断会返回-ERESTARTSYS信号，
否则在满足condition条件时返回0，期间没有时间限制。

```c
#define wait_event_interruptible(wq, condition)	
({				
	int __ret = 0;		
	if (!(condition))//如果条件不满足，调用等待队列
		__wait_event_interruptible(wq, condition, __ret);
	__ret;					
})

#define __wait_event_interruptible(wq, condition, ret)		
do {							
	DEFINE_WAIT(__wait);//定义一个等待线程
	for (;;) {						
		prepare_to_wait(&wq, &__wait, TASK_INTERRUPTIBLE);
		if (condition)				
			break;				
		if (!signal_pending(current)) {		
			schedule();				
			continue;				
		}						
		ret = -ERESTARTSYS;				
		break;						
	}						
	finish_wait(&wq, &__wait);//把唤醒的进程从等待队列中去掉，并设置当前状态为TASK_RUNNING
} while (0)
```

#### 可中断，有超时限制的wait函数

该方法若超时结束会返回0，中断结束返回-ERESTARTSYS信号，否则在满足条件时返回剩余可等待时间。

```c
#define wait_event_interruptible_timeout(wq, condition, timeout)
({							
	long __ret = timeout;				
	if (!(condition))			
		__wait_event_interruptible_timeout(wq, condition, __ret); 
	__ret;						
})

#define __wait_event_interruptible_timeout(wq, condition, ret)	
do {									
	DEFINE_WAIT(__wait);						
									
	for (;;) {							
		prepare_to_wait(&wq, &__wait, TASK_INTERRUPTIBLE);	
		if (condition)						
			break;						
		if (!signal_pending(current)) {				
			ret = schedule_timeout(ret);			
			if (!ret)					
				break;					
			continue;					
		}							
		ret = -ERESTARTSYS;					
		break;							
	}								
	finish_wait(&wq, &__wait);					
} while (0)
```

### iowait

什么是iowait？ 顾名思义，就是系统因为io导致的进程wait。

这时候系统在做io，导致没有进程在干活，cpu在执行idle进程空转，

所以说iowait的产生要满足两个条件，一是进程在等io，二是等io时没有进程可运行。


#### 可中断无超时iowait函数

```c

#define __wait_io_event_interruptible(wq, condition, ret)		
do {									
	DEFINE_WAIT(__wait);						
									
	for (;;) {							
		prepare_to_wait(&wq, &__wait, TASK_INTERRUPTIBLE);	
		if (condition)						
			break;						
		if (!signal_pending(current)) {				
			io_schedule();					
			continue;					
		}							
		ret = -ERESTARTSYS;					
		break;							
	}					
	finish_wait(&wq, &__wait);			
} while (0)

```


#### 有中断有超时iowait函数

```c
#define __wait_io_event_interruptible_timeout(wq, condition, ret)	
do {									
	DEFINE_WAIT(__wait);						
									
	for (;;) {							
		prepare_to_wait(&wq, &__wait, TASK_INTERRUPTIBLE);	
		if (condition)						
			break;						
		if (!signal_pending(current)) {				
			ret = io_schedule_timeout(ret);			
			if (!ret)					
				break;					
			continue;					
		}							
		ret = -ERESTARTSYS;					
		break;							
	}								
	finish_wait(&wq, &__wait);					
} while (0)
```


### 互斥wait函数


正常情况下，当一个进程调用一个等待队列的wake_up函数时，所有这个队列上的进程都被置为可运行的，然而在许多情况下，
我们提前知道只有一个进程将被唤醒并能成功的使用资源，其余被被唤醒的进程将再次进入等待，由此看来这种行为会降低系统的性能。因为内核开发者开发了一种“互斥等待”添加到内核中，与普通wait函数不同的是：

1、当一个进程有WQ_FLAG_EXCLUSEVE标志时，它被添加到等待队列的队尾，否则添加到对头。
2、当一个队列上的wake_up函数被调到用时，在唤醒第一个带有WQ_FLAG_EXCLUSEVE标志的进程后停止，但内核仍会调用所有非互斥等待的进程，因为这些进程排在队首，互斥进程排在队尾，而唤醒的顺序是由首到尾。

最后的结果是以顺序的方式唤醒第一个互斥的进程后停止唤醒后面的进程。

使用互斥等待需要满足三个条件：
1、希望该进程对资源进行有效竞争，
2、当资源可用时唤醒一个进程就足够完全消耗资源，
3、所有使用该资源的进程都应该统一的使用互斥等待。

#### 可中断，无超时限制的互斥wait函数

```c
#define __wait_event_interruptible_exclusive(wq, condition, ret)	
do {									
	DEFINE_WAIT(__wait);						
									
	for (;;) {							
		prepare_to_wait_exclusive(&wq, &__wait,			
					TASK_INTERRUPTIBLE);		
		if (condition) {					
			finish_wait(&wq, &__wait);			
			break;						
		}							
		if (!signal_pending(current)) {				
			schedule();					
			continue;					
		}							
		ret = -ERESTARTSYS;					
		abort_exclusive_wait(&wq, &__wait, 	//把该进程的状态回复成running，并从等待队列中删除		
				TASK_INTERRUPTIBLE, NULL);		
		break;							
	}		
```

## wake_up 函数

wake_up函数用于唤醒等待队列上的进程，使其重新进入就绪状态。

```c
// 唤醒非中断的一个进程
#define wake_up(x)  __wake_up(x, TASK_NORMAL, 1, NULL)
// 唤醒非中断的nr个进程
#define wake_up_nr(x, nr)  __wake_up(x, TASK_NORMAL, nr, NULL)
// 唤醒非中断的所有进程
#define wake_up_all(x)  __wake_up(x, TASK_NORMAL, 0, NULL)
// 唤醒可中断的一个进程
#define wake_up_interruptible(x)  __wake_up(x, TASK_INTERRUPTIBLE, 1, NULL)
// 唤醒可中断的nr个进程
#define wake_up_interruptible_nr(x, nr)  __wake_up(x, TASK_INTERRUPTIBLE, nr, NULL)
// 唤醒可中断的所有进程
#define wake_up_interruptible_all(x)  __wake_up(x, TASK_INTERRUPTIBLE, 0, NULL)
```




> https://www.cnblogs.com/schips/p/linux_driver_wait_and_wait_queue.html
