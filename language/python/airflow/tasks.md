# tasks
运行单个实例
Run a single task instance

```shell
usage: airflow tasks run [-h] [--cfg-path CFG_PATH] [--error-file ERROR_FILE]
                         [-f] [-A] [-i] [-I] [-N] [-l] [-m] [-p PICKLE]
                         [--pool POOL] [--ship-dag] [-S SUBDIR]
                         dag_id task_id execution_date_or_run_id
```


positional arguments:
  dag_id                The id of the dag
  task_id               The id of the task
  execution_date_or_run_id
                        The execution_date of the DAG or run_id of the DAGRun

optional arguments:
  -h, --help            show this help message and exit
  --cfg-path CFG_PATH   Path to config file to use instead of airflow.cfg
  --error-file ERROR_FILE
                        File to store task failure error
  -f, --force           Ignore previous task instance state, rerun regardless if task already succeeded/failed
  -A, --ignore-all-dependencies
                        Ignores all non-critical dependencies, including ignore_ti_state and ignore_task_deps
  -i, --ignore-dependencies
                        Ignore task-specific dependencies, e.g. upstream, depends_on_past, and retry delay dependencies
  -I, --ignore-depends-on-past
                        Ignore depends_on_past dependencies (but respect upstream dependencies)
  -N, --interactive     Do not capture standard output and error streams (useful for interactive debugging)
  -l, --local           Run the task using the LocalExecutor
  -m, --mark-success    Mark jobs as succeeded without running them
  -p PICKLE, --pickle PICKLE
                        Serialized pickle object of the entire dag (used internally)
  --pool POOL           Resource pool to use
  --ship-dag            Pickles (serializes) the DAG and ships it to the worker
  -S SUBDIR, --subdir SUBDIR
                        File location or directory from which to look for the dag. Defaults to '[AIRFLOW_HOME]/dags' where [AIRFLOW_HOME] is the value you set for 'AIRFLOW_HOME' config you set in 'airflow.cfg'
