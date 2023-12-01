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

- dag_run.schedule_tis
```shell
SELECT task_instance.try_number           AS task_instance_try_number,
       task_instance.task_id              AS task_instance_task_id,
       task_instance.dag_id               AS task_instance_dag_id,
       task_instance.run_id               AS task_instance_run_id,
       task_instance.start_date           AS task_instance_start_date,
       task_instance.end_date             AS task_instance_end_date,
       task_instance.duration             AS task_instance_duration,
       task_instance.state                AS task_instance_state,
       task_instance.max_tries            AS task_instance_max_tries,
       task_instance.hostname             AS task_instance_hostname,
       task_instance.unixname             AS task_instance_unixname,
       task_instance.job_id               AS task_instance_job_id,
       task_instance.pool                 AS task_instance_pool,
       task_instance.pool_slots           AS task_instance_pool_slots,
       task_instance.queue                AS task_instance_queue,
       task_instance.priority_weight      AS task_instance_priority_weight,
       task_instance.operator             AS task_instance_operator,
       task_instance.queued_dttm          AS task_instance_queued_dttm,
       task_instance.queued_by_job_id     AS task_instance_queued_by_job_id,
       task_instance.pid                  AS task_instance_pid,
       task_instance.executor_config      AS task_instance_executor_config,
       task_instance.external_executor_id AS task_instance_external_executor_id,
       task_instance.trigger_id           AS task_instance_trigger_id,
       task_instance.trigger_timeout      AS task_instance_trigger_timeout,
       task_instance.next_method          AS task_instance_next_method,
       task_instance.next_kwargs          AS task_instance_next_kwargs,
       dag_run_1.state                    AS dag_run_1_state,
       dag_run_1.id                       AS dag_run_1_id,
       dag_run_1.dag_id                   AS dag_run_1_dag_id,
       dag_run_1.queued_at                AS dag_run_1_queued_at,
       dag_run_1.execution_date           AS dag_run_1_execution_date,
       dag_run_1.start_date               AS dag_run_1_start_date,
       dag_run_1.end_date                 AS dag_run_1_end_date,
       dag_run_1.run_id                   AS dag_run_1_run_id,
       dag_run_1.creating_job_id          AS dag_run_1_creating_job_id,
       dag_run_1.external_trigger         AS dag_run_1_external_trigger,
       dag_run_1.run_type                 AS dag_run_1_run_type,
       dag_run_1.conf                     AS dag_run_1_conf,
       dag_run_1.data_interval_start      AS dag_run_1_data_interval_start,
       dag_run_1.data_interval_end        AS dag_run_1_data_interval_end,
       dag_run_1.last_scheduling_decision AS dag_run_1_last_scheduling_decision,
       dag_run_1.dag_hash                 AS dag_run_1_dag_hash
FROM task_instance
         INNER JOIN dag_run AS dag_run_1
                    ON dag_run_1.dag_id = task_instance.dag_id AND dag_run_1.run_id = task_instance.run_id
WHERE task_instance.dag_id = % (dag_id_1)s AND task_instance.run_id = %(run_id_1)s AND task_instance.state != %(state_1)s AND task_instance.task_id IN (%(task_id_1)s)
```


- 忽略依赖
```shell
<!-- 默认不提供 -->
airflow/jobs/scheduler_job.py:_enqueue_task_instances_with_queued_state:ti.command_as_list
```
