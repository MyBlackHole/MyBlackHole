- 僵尸进程检测 sql
```sql
SELECT job.latest_heartbeat
     , job.state
     , task_instance.try_number
     , task_instance.task_id
     , task_instance.dag_id
     , task_instance.run_id
     , task_instance.start_date
     , task_instance.end_date
     , task_instance.duration
     , task_instance.state
     , task_instance.max_tries, task_instance.hostname
     , task_instance.unixname
     , task_instance.job_id
     , task_instance.pool
     , task_instance.pool_slots
     , task_instance.queue
     , task_instance.priority_weight
     , task_instance.operator
     , task_instance.queued_dttm
     , task_instance.queued_by_job_id, task_instance.pid
     , task_instance.executor_config
     , task_instance.external_executor_id
     , task_instance.trigger_id
     , task_instance.trigger_timeout
     , task_instance.next_method
     , task_instance.next_kwargs
     , dag.fileloc
     , dag_run_1.state, dag_run_1.id
     , dag_run_1.dag_id
     , dag_run_1.queued_at
     , dag_run_1.execution_date
     , dag_run_1.start_date
     , dag_run_1.end_date
     , dag_run_1.run_id
     , dag_run_1.creating_job_id
     , dag_run_1.external_trigger
     , dag_run_1.run_type
     , dag_run_1.conf
     , dag_run_1.data_interval_start
     , dag_run_1.data_interval_end
     , dag_run_1.last_scheduling_decision
     , dag_run_1.dag_hash
FROM task_instance
         JOIN job ON task_instance.job_id = job.id AND job.job_type IN ('LocalTaskJob')
         JOIN dag ON task_instance.dag_id = dag.dag_id
         JOIN dag_run AS dag_run_1
              ON dag_run_1.dag_id = task_instance.dag_id AND dag_run_1.run_id = task_instance.run_id
WHERE task_instance.state = 'running' AND (job.state != 'running' OR job.latest_heartbeat < '2023-05-19 04:37:33.075284')
```


Process ForkProcess-1:
Traceback (most recent call last):
  File "/usr/lib64/python3.6/multiprocessing/process.py", line 258, in _bootstrap
    self.run()
  File "/usr/lib64/python3.6/multiprocessing/process.py", line 93, in run
    self._target(*self._args, **self._kwargs)
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/manager.py", line 287, in _run_processor_manager
    processor_manager.start()
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/manager.py", line 520, in start
    return self._run_parsing_loop()
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/manager.py", line 611, in _run_parsing_loop
    self.collect_results()
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/manager.py", line 939, in collect_results
    self._collect_results_from_processor(processor)
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/manager.py", line 908, in _collect_results_from_processor
    if processor.result is not None:
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/processor.py", line 322, in result
    if not self.done:
  File "/opt/aio/airflow/lib64/python3.6/site-packages/airflow/dag_processing/processor.py", line 287, in done
    if self._parent_channel.poll():
  File "/usr/lib64/python3.6/multiprocessing/connection.py", line 259, in poll
    self._check_closed()
  File "/usr/lib64/python3.6/multiprocessing/connection.py", line 140, in _check_closed
    raise OSError("handle is closed")
OSError: handle is closed
