# celery executors

celery_executor.py
airflow/executors/celery_executor.py:execute_command
    airflow/executors/celery_executor.py:_execute_in_fork
['airflow', 'tasks', 'run', 'storage_size_update', '更新存储节点容量', 'manual__2023-03-21T05:31:00.573310+00:00', '--local', '--subdir', 'DAGS_FOLDER/storage_size_update.py']
    airflow/cli/commands/task_command.py:task_run
        airflow/cli/commands/task_command.py:_run_task_by_selected_method
            airflow/cli/commands/task_command.py:_run_task_by_local_task_job
                LocalTaskJob.(BaseJob).run
                    LocalTaskJob._execute
                        TaskInstance.check_and_change_state_before_execution
                        StandardTaskRunner.start
                        :   [fork] -- StandardTaskRunner._start_by_fork
                        :       standard_task_runner.py
                        :       ['airflow', 'tasks', 'run', 'test_dag', 'test1', 'manual__2023-05-16T09:45:26.465185+00:00', '--job-id', '376', '--raw', '--subdir', 'DAGS_FOLDER/test_dag.py', '--cfg-path', '/tmp/tmpqtb52lv9', '--error-file', '/tmp/tmpwrv2dtux']
                        :           airflow/cli/commands/task_command.py:task_run
                        :               airflow/cli/commands/task_command.py:_run_task_by_selected_method
                        :                   airflow/cli/commands/task_command.py:_run_raw_task
                        :                       TaskInstance._run_raw_task
                        :                           TaskInstance._execute_task_with_callbacks
                        :                               TaskInstance._execute_task
                        :                                   TaskInstance.task.execute(context)
                        :
                        LocalTaskJob [while not self.terminating]
                            LocalTaskJob.(BaseJob).heartbeat
