# __all_virtual_table

```shell
+--------------------------------------+---------------------+------+-----+--------------------+-------+
| Field                                | Type                | Null | Key | Default            | Extra |
+--------------------------------------+---------------------+------+-----+--------------------+-------+
| tenant_id                            | bigint(20)          | NO   | PRI | <null>             |       |
| table_id                             | bigint(20)          | NO   | PRI | <null>             |       |
| gmt_create                           | timestamp(6)        | NO   |     | <null>             |       |
| gmt_modified                         | timestamp(6)        | NO   |     | <null>             |       |
| table_name                           | varchar(256)        | NO   |     |                    |       |
| database_id                          | bigint(20)          | NO   |     | <null>             |       |
| table_type                           | bigint(20)          | NO   |     | <null>             |       |
| load_type                            | bigint(20)          | NO   |     | <null>             |       |
| def_type                             | bigint(20)          | NO   |     | <null>             |       |
| rowkey_column_num                    | bigint(20)          | NO   |     | <null>             |       |
| index_column_num                     | bigint(20)          | NO   |     | <null>             |       |
| max_used_column_id                   | bigint(20)          | NO   |     | <null>             |       |
| replica_num                          | bigint(20)          | NO   |     | <null>             |       |
| autoinc_column_id                    | bigint(20)          | NO   |     | <null>             |       |
| auto_increment                       | bigint(20) unsigned | YES  |     | 1                  |       |
| read_only                            | bigint(20)          | NO   |     | <null>             |       |
| rowkey_split_pos                     | bigint(20)          | NO   |     | <null>             |       |
| compress_func_name                   | varchar(128)        | NO   |     | <null>             |       |
| expire_condition                     | varchar(4096)       | NO   |     | <null>             |       |
| is_use_bloomfilter                   | bigint(20)          | NO   |     | <null>             |       |
| comment                              | varchar(4096)       | NO   |     |                    |       |
| block_size                           | bigint(20)          | NO   |     | <null>             |       |
| collation_type                       | bigint(20)          | NO   |     | <null>             |       |
| data_table_id                        | bigint(20)          | YES  |     | <null>             |       |
| index_status                         | bigint(20)          | NO   |     | <null>             |       |
| tablegroup_id                        | bigint(20)          | NO   |     | <null>             |       |
| progressive_merge_num                | bigint(20)          | NO   |     | <null>             |       |
| index_type                           | bigint(20)          | NO   |     | <null>             |       |
| part_level                           | bigint(20)          | NO   |     | <null>             |       |
| part_func_type                       | bigint(20)          | NO   |     | <null>             |       |
| part_func_expr                       | varchar(4096)       | NO   |     | <null>             |       |
| part_num                             | bigint(20)          | NO   |     | <null>             |       |
| sub_part_func_type                   | bigint(20)          | NO   |     | <null>             |       |
| sub_part_func_expr                   | varchar(4096)       | NO   |     | <null>             |       |
| sub_part_num                         | bigint(20)          | NO   |     | <null>             |       |
| create_mem_version                   | bigint(20)          | NO   |     | <null>             |       |
| schema_version                       | bigint(20)          | NO   |     | <null>             |       |
| view_definition                      | longtext            | NO   |     | <null>             |       |
| view_check_option                    | bigint(20)          | NO   |     | <null>             |       |
| view_is_updatable                    | bigint(20)          | NO   |     | <null>             |       |
| zone_list                            | varchar(8192)       | NO   |     | <null>             |       |
| primary_zone                         | varchar(128)        | YES  |     | <null>             |       |
| index_using_type                     | bigint(20)          | NO   |     | 0                  |       |
| parser_name                          | varchar(128)        | YES  |     | <null>             |       |
| index_attributes_set                 | bigint(20)          | YES  |     | 0                  |       |
| locality                             | varchar(4096)       | NO   |     |                    |       |
| tablet_size                          | bigint(20)          | NO   |     | 134217728          |       |
| pctfree                              | bigint(20)          | NO   |     | 10                 |       |
| previous_locality                    | varchar(4096)       | NO   |     |                    |       |
| max_used_part_id                     | bigint(20)          | NO   |     | -1                 |       |
| partition_cnt_within_partition_table | bigint(20)          | NO   |     | -1                 |       |
| partition_status                     | bigint(20)          | YES  |     | 0                  |       |
| partition_schema_version             | bigint(20)          | YES  |     | 0                  |       |
| max_used_constraint_id               | bigint(20)          | YES  |     | <null>             |       |
| session_id                           | bigint(20)          | YES  |     | 0                  |       |
| pk_comment                           | varchar(4096)       | NO   |     |                    |       |
| sess_active_time                     | bigint(20)          | YES  |     | 0                  |       |
| row_store_type                       | varchar(128)        | YES  |     | encoding_row_store |       |
| store_format                         | varchar(128)        | YES  |     |                    |       |
| duplicate_scope                      | bigint(20)          | YES  |     | 0                  |       |
| binding                              | tinyint(4)          | YES  |     | 0                  |       |
| progressive_merge_round              | bigint(20)          | YES  |     | 0                  |       |
| storage_format_version               | bigint(20)          | YES  |     | 2                  |       |
| table_mode                           | bigint(20)          | NO   |     | 0                  |       |
| encryption                           | varchar(128)        | YES  |     |                    |       |
| tablespace_id                        | bigint(20)          | NO   |     | -1                 |       |
| drop_schema_version                  | bigint(20)          | NO   |     | -1                 |       |
| is_sub_part_template                 | tinyint(4)          | NO   |     | 1                  |       |
| dop                                  | bigint(20)          | NO   |     | 1                  |       |
| character_set_client                 | bigint(20)          | NO   |     | 0                  |       |
| collation_connection                 | bigint(20)          | NO   |     | 0                  |       |
| auto_part_size                       | bigint(20)          | NO   |     | -1                 |       |
| auto_part                            | tinyint(4)          | NO   |     | 0                  |       |
| sub_part_template                    | longtext            | YES  |     | <null>             |       |
+--------------------------------------+---------------------+------+-----+--------------------+-------+
```

- 查询 table_id
```sql
select table_id, table_name from oceanbase.__all_virtual_table;

+---------------+---------------------------------------------------------+
| table_id      | table_name                                              |
+---------------+---------------------------------------------------------+
| 1099511627877 | __all_meta_table                                        |
| 1099511627878 | __all_user                                              |
| 1099511627879 | __all_user_history                                      |
| 1099511627880 | __all_database                                          |
| 1099511627881 | __all_database_history                                  |
| 1099511627882 | __all_tablegroup                                        |
| 1099511627883 | __all_tablegroup_history                                |
| 1099511627884 | __all_tenant                                            |
| 1099511627885 | __all_tenant_history                                    |
| 1099511627886 | __all_table_privilege                                   |
| 1099511627887 | __all_table_privilege_history                           |
| 1099511627888 | __all_database_privilege                                |
| 1099511627889 | __all_database_privilege_history                        |
| 1099511627890 | __all_table_history                                     |
| 1099511627891 | __all_column_history                                    |
| 1099511627892 | __all_zone                                              |
| 1099511627893 | __all_server                                            |
| 1099511627894 | __all_sys_parameter                                     |
| 1099511627895 | __tenant_parameter                                      |
......
| 1099511655886 | DBA_SCHEDULER_JOBS                                      |
| 1099511655887 | DBA_SCHEDULER_PROGRAM                                   |
| 1099511677777 | article                                                 |
+---------------+---------------------------------------------------------+
```
