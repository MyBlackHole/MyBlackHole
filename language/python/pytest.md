# pytest

```shell
python3.6 -m pdb /home/black/.pyenv/versions/3.6.8/envs/venv-airflow/bin/pytest tests/jobs/test_local_task_job.py::TestLocalTaskJob::test_heartbeat_failed_fast

python3.6 -m pdb /home/black/.pyenv/versions/default/bin/pytest tests/dag_processing/test_processor.py::TestDagFileProcessor::test_dag_file_processor_sla_miss_callback
```

测试单个
```shell
pytest tests/dag_processing/test_processor.py::TestDagFileProcessor::test_dag_file_processor_sla_miss_callback
```
