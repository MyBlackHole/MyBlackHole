# runq

CPU上的运行队列情况


```shell
crash> runq
CPU 0 RUNQUEUE: ffff955b3ec22700
  CURRENT: PID: 0      TASK: ffffffffb9a12780  COMMAND: "swapper/0"
  RT PRIO_ARRAY: ffff955b3ec22940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3ec227b0
     [no tasks queued]

CPU 1 RUNQUEUE: ffff955b3ec62700
  CURRENT: PID: 2332295  TASK: ffff953c5b23c680  COMMAND: "easymonitor_mai"
  RT PRIO_ARRAY: ffff955b3ec62940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3ec627b0
     [no tasks queued]

CPU 2 RUNQUEUE: ffff955b3eca2700
  CURRENT: PID: 0      TASK: ffff953c58a9de00  COMMAND: "swapper/2"
  RT PRIO_ARRAY: ffff955b3eca2940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3eca27b0
     [no tasks queued]

CPU 3 RUNQUEUE: ffff955b3ece2700
  CURRENT: PID: 0      TASK: ffff953c58a98000  COMMAND: "swapper/3"
  RT PRIO_ARRAY: ffff955b3ece2940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3ece27b0
     [no tasks queued]

CPU 4 RUNQUEUE: ffff955b3ed22700
  CURRENT: PID: 0      TASK: ffff953c58a99780  COMMAND: "swapper/4"
  RT PRIO_ARRAY: ffff955b3ed22940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3ed227b0
     [no tasks queued]

CPU 5 RUNQUEUE: ffff955b3ed62700
  CURRENT: PID: 0      TASK: ffff953c58a9af00  COMMAND: "swapper/5"
  RT PRIO_ARRAY: ffff955b3ed62940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3ed627b0
     [no tasks queued]

CPU 6 RUNQUEUE: ffff955b3eda2700
  CURRENT: PID: 0      TASK: ffff953c58a9c680  COMMAND: "swapper/6"
  RT PRIO_ARRAY: ffff955b3eda2940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3eda27b0
     [no tasks queued]

CPU 7 RUNQUEUE: ffff955b3ede2700
  CURRENT: PID: 0      TASK: ffff953c58ab2f00  COMMAND: "swapper/7"
  RT PRIO_ARRAY: ffff955b3ede2940
     [no tasks queued]
  CFS RB_ROOT: ffff955b3ede27b0
     [no tasks queued]

CPU 8 RUNQUEUE: ffff956b7f822700
  CURRENT: PID: 0      TASK: ffff955bc0054680  COMMAND: "swapper/8"
  RT PRIO_ARRAY: ffff956b7f822940
     [no tasks queued]
  CFS RB_ROOT: ffff956b7f8227b0
     [no tasks queued]

CPU 9 RUNQUEUE: ffff956b7f862700
  CURRENT: PID: 0      TASK: ffff955bc0055e00  COMMAND: "swapper/9"
  RT PRIO_ARRAY: ffff956b7f862940
     [no tasks queued]
  CFS RB_ROOT: ffff956b7f8627b0
     [no tasks queued]

crash> runq -m
 CPU 0: [0 00:00:00.000]  PID: 965    TASK: ffff9c5a03c2bc00  COMMAND: "irqbalance"
 CPU 1: [0 02:30:16.843]  PID: 0      TASK: ffff9c5a01e73c00  COMMAND: "swapper/1"
 CPU 2: [0 02:30:16.835]  PID: 0      TASK: ffff9c5a01e71e00  COMMAND: "swapper/2"
 CPU 3: [0 02:30:16.873]  PID: 0      TASK: ffff9c5a01e70000  COMMAND: "swapper/3"
```
