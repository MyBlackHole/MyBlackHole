
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
