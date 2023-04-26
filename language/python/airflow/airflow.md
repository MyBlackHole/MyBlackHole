# airflow
python 编写的 data pipeline 调度和监控工作流平台
Airflow 是通过 DAG（Directed acyclic graph 有向无环图）来管理任务流程的任务调度工具
该工具还提供了一个基于 Web 的用户界面可以可视化管道的依赖关系、监控进度、触发任务等

- 解决那些问题
    在一个运维系统，数据分析系统，或测试系统等大型系统中，我们会有各种各样的依赖需求
    包括但不限于： 
    - 时间依赖
        任务需要等待某一个时间点触发
        airflow 实现的 crontab 可以很好地处理定时执行任务的需求，但仅能管理时间上的依赖
    - 外部系统依赖
        任务依赖外部系统需要调用接口去访问
    - 任务间依赖
        任务 A 需要在任务 B 完成后启动，两个任务互相间会产生影响
        DAG 解决了任务间的依赖问题
    - 资源环境依赖
        任务消耗资源非常多， 或者只能在特定的机器上执行

## 架构
![[imgs/Pasted image 20230418095554.png]]
- metastore (元数据)
    这个数据库存储有关任务状态的信息
- scheduler (调度器)
    使用 DAG 定义结合元数据的任务状态来决定哪些任务需要被执行以及任务执行优先级的过程
- Executor (执行器)
    是一个消息队列进程，它被绑定到调度器中，用于确定实际执行每个任务计划的工作进程
    有不同类型的执行器，每个执行器都使用一个指定工作进程的类来执行任务。 例如，LocalExecutor 使用与调度器进程在同一台机器上运行的并行进程执行任务
    其他像 CeleryExecutor 的执行器使用存在于独立的工作机器集群中的工作进程执行任务
- Workers (工人)
    实际执行任务逻辑的进程，由正在使用的执行器确定

## 配置
```shell
# export AIRFLOW_DB_HOSTNAME=192.168.78.212
# export AIRFLOW_DB_PORT=3306
# export AIRFLOW_DB_USERNAME=root
# export AIRFLOW_DB_PASSWORD=p@3Sw0rd
```

## status
[[status]]

## 常用命令
- 打印当前 airflow 配置
```shell
airflow info
```

- 测试 db 连接
```shell
airflow db check
```

- 查看自定义 dag 
```shell
airflow dags list
```

- 测试任务，格式：airflow test dag_id task_id execution_time
```shell
airflow test test_task test1 2019-09-10
```

- 开始运行任务(这一步也可以在web界面点trigger按钮)
```shell
airflow trigger_dag test_task
```

- 守护进程运行webserver, 默认端口为8080，也可以通过`-p`来指定
```shell
airflow webserver -D  
airflow webserver -D --debug
```

- 守护进程运行调度器     
```shell
airflow scheduler -D   
```

- 守护进程运行调度器    
```shell
airflow worker -D          
```

- 暂停任务
```shell
airflow pause dag_id　     
```

- 取消暂停，等同于在web管理界面打开off按钮
```shell
airflow unpause dag_id     
```

- 查看task列表
```shell
airflow list_tasks dag_id  查看task列表
```

- 初始化 airflow 元数据库
```shell
airflow initdb
```

- 清空任务状态
```shell
airflow clear dag_id       
```

- 运行task
```shell
airflow run dag_id task_id execution_date
```

- 重启 airflow
```shell
# 结束 airflow 相关进程
ps -ef|grep 'airflow'|grep -v grep|awk '{print $2}'|xargs kill -9

# 删除 pid
rm -rf airflow-*
 或者
rm -rf airflow-*pid

# 启动
airflow scheduler -D --debug
airflow webserver -D --debug
# 启动 worker
airflow celery worker -D
```

- 暂停任务
```shell
airflow dags pause test_task
```

- 清空任务实例
```shell
airflow clear dag_id
```
