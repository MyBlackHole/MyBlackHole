celery_executor 为例子

airflow scheduler:
airflow/cli/commands/scheduler_command.py:scheduler
    airflow/cli/commands/scheduler_command.py:_run_scheduler_job
        airflow/cli/commands/scheduler_command.py:_create_scheduler_job
            airflow/cli/commands/scheduler_command.py:SchedulerJob(BaseJob)
        airflow/jobs/base_job.py:BaseJob.run
            airflow/jobs/scheduler_job.py:SchedulerJob._execute
                airflow/jobs/scheduler_job.py:SchedulerJob.processor_agent.start (文件监控)
                    airflow/dag_processing/manager.py:context.Process (起进程监控)
                airflow/jobs/scheduler_job.py:SchedulerJob._run_scheduler_loop
                    airflow/jobs/scheduler_job.py:SchedulerJob._run_scheduler_loop:itertools.count(start=1)(无限循环)
                        airflow/jobs/scheduler_job.py:SchedulerJob._do_scheduling
                            airflow/jobs/scheduler_job.py:SchedulerJob._create_dagruns_for_dags
                                airflow/models/dag.py:DagModel.dags_needing_dagruns (获取需要创建 dag_run 的 dag, NUM_DAGS_PER_DAGRUN_QUERY 个(会对查询的 dag 加行锁))
                                airflow/jobs/scheduler_job.py:SchedulerJob._create_dag_runs (根据上一行的 dags 创建 dag_run, 初始化运行类型为 DagRunType.SCHEDULED)
                            airflow/jobs/scheduler_job.py:SchedulerJob._schedule_dag_run 
                                airflow/models/dagrun.py:DagRun.schedule_tis
                            airflow/jobs/scheduler_job.py:SchedulerJob._critical_section_execute_task_instances (发起任务实例调度 找 SCHEDULED 的 TI)
                                airflow/jobs/scheduler_job.py:SchedulerJob._executable_task_instances_to_queued(max_tis) (发起任务实例调度, 指定最大量, 对需要调度任务改状态未 State.QUEUED)
                        airflow/jobs/scheduler_job.py:SchedulerJob.processor_agent.heartbeat (看看他还健在吗)
                        airflow/executors/base_executor.py:SchedulerJob.executor(CeleryExecutor(BaseExecutor)).heartbeat (执行器心跳)
                            airflow/executors/celery_executor.py:CeleryExecutor.trigger_tasks (触发任务实例)
                                airflow/executors/celery_executor.py:CeleryExecutor._process_tasks (发送任务)
                                    airflow/executors/celery_executor.py:CeleryExecutor._send_tasks_to_celery (发送任务)
                                        airflow/executors/celery_executor.py:ProcessPoolExecutor:send_task_to_executor (启用进程池发送任务)
                        airflow/jobs/scheduler_job.py:SchedulerJob.heartbeat (心跳)
