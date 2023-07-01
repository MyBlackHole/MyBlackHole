# 

    """
    Trying to guess the debugger used by the user. When it doesn't find any user-installed debugger,
    returns ``pdb``.

    List of supported debuggers:

    * `pudb <https://github.com/inducer/pudb>`__
    * `web_pdb <https://github.com/romanvm/python-web-pdb>`__
    * `ipdb <https://github.com/gotcha/ipdb>`__
    * `pdb <https://docs.python.org/3/library/pdb.html>`__
    """

## get_logs_with_metadata

- 获取源数据
```shell
www/views.py:1186
    logs, metadatas = self.log_handler.read(ti, try_number, metadata=metadata)
        utils/log/file_task_handler.py(206)read()


```

- 直接运行 dag
```shell
  /opt/aio/airflow/bin/airflow(8)<module>()
-> sys.exit(main())
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/__main__.py(48)main()
-> args.func(args)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/cli_parser.py(48)command()
-> return func(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/cli.py(92)wrapper()
-> return f(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(282)task_run()
-> dag = get_dag(args.subdir, args.dag_id)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/cli.py(190)get_dag()
-> dagbag = DagBag(process_subdir(subdir))
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/models/dagbag.py(141)__init__()
-> safe_mode=safe_mode,
> /opt/aio/airflow/lib64/python3.6/site-packages/airflow/models/dagbag.py(502)collect_dags()
-> self.log.info("Filling up the DagBag from %s", dag_folder)


  /opt/aio/airflow/bin/airflow(8)<module>()
-> sys.exit(main())
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/__main__.py(48)main()
-> args.func(args)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/cli_parser.py(48)command()
-> return func(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/cli.py(92)wrapper()
-> return f(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(299)task_run()
-> _run_task_by_selected_method(args, dag, ti)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(106)_run_task_by_selected_method()
-> if args.local:
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(165)_run_task_by_local_task_job()
-> run_job.run()
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/jobs/base_job.py(245)run()
-> self._execute()
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/jobs/local_task_job.py(97)_execute()
-> external_executor_id=self.external_executor_id,
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/session.py(70)wrapper()
-> return func(*args, session=session, **kwargs)
> /opt/aio/airflow/lib64/python3.6/site-packages/airflow/models/taskinstance.py(1172)check_and_change_state_before_execution()



  /opt/aio/airflow/bin/airflow(8)<module>()
-> sys.exit(main())
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/__main__.py(48)main()
-> args.func(args)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/cli_parser.py(48)command()
-> return func(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/cli.py(92)wrapper()
-> return f(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(299)task_run()
-> _run_task_by_selected_method(args, dag, ti)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(107)_run_task_by_selected_method()
-> _run_task_by_local_task_job(args, ti)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/cli/commands/task_command.py(165)_run_task_by_local_task_job()
-> run_job.run()
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/jobs/base_job.py(245)run()
-> self._execute()
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/jobs/local_task_job.py(97)_execute()
-> external_executor_id=self.external_executor_id,
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/session.py(70)wrapper()
-> return func(*args, session=session, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/models/taskinstance.py(1172)check_and_change_state_before_execution()
-> task = self.task
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/utils/session.py(67)wrapper()
-> return func(*args, **kwargs)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/models/taskinstance.py(1019)are_dependencies_met()
-> for dep_status in self.get_failed_dep_statuses(dep_context=dep_context, session=session):
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/models/taskinstance.py(1040)get_failed_dep_statuses()
-> for dep_status in dep.get_dep_statuses(self, session, dep_context):
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/ti_deps/deps/base_ti_dep.py(101)get_dep_statuses()
-> yield from self._get_dep_statuses(ti, session, dep_context)
  /opt/aio/airflow/lib64/python3.6/site-packages/airflow/ti_deps/deps/trigger_rule_dep.py(80)_get_dep_statuses()
-> session=session,
> /opt/aio/airflow/lib64/python3.6/site-packages/airflow/ti_deps/deps/trigger_rule_dep.py(129)_evaluate_trigger_rule()
-> if flag_upstream_failed:
```



                self._try_number += 1 
                self.error(session)
