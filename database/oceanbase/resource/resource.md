# resource

CPU 信息：
__all_virtual_server_stat：CPU 总核数、已分配核数。

内存信息
__all_virtual_server_stat：内存总大小、已使用大小。

磁盘信息
__all_virtual_disk_stat：磁盘总大小、已使用信息。

```shell
select a.zone,concat(a.svr_ip,':',a.svr_port) observer, cpu_total, cpu_assigned, (cpu_total-cpu_assigned) cpu_free, mem_total/1024/1024/1024 mem_total_gb, mem_assigned/1024/1024/1024 mem_assign_gb, (mem_total-mem_assigned)/1024/1024/1024 mem_free_gb 
from __all_virtual_server_stat a join __all_server b on (a.svr_ip=b.svr_ip and a.svr_port=b.svr_port)
order by a.zone, a.svr_ip;
+-------+---------------------+-----------+--------------+----------+-----------------+----------------+-----------------+
| zone  | observer            | cpu_total | cpu_assigned | cpu_free | mem_total_gb    | mem_assign_gb  | mem_free_gb     |
+-------+---------------------+-----------+--------------+----------+-----------------+----------------+-----------------+
| zone1 | 192.168.80.143:2882 | 12.0      | 6.5          | 5.5      | 18.000000000000 | 9.500000000000 | 8.500000000000  |
| zone2 | 192.168.80.144:2882 | 12.0      | 6.5          | 5.5      | 18.000000000000 | 9.500000000000 | 8.500000000000  |
| zone3 | 192.168.80.145:2882 | 12.0      | 2.5          | 9.5      | 18.000000000000 | 4.500000000000 | 13.500000000000 |
+-------+---------------------+-----------+--------------+----------+-----------------+----------------+-----------------+


select (cpu_total-cpu_assigned) cpu_free, (mem_total-mem_assigned)/1024/1024/1024 mem_free_gb, (disk_total-disk_assigned)/1024/1024/1024 disk_free from __all_virtual_server_stat;
select (cpu_total-cpu_assigned) cpu_free, (mem_total-mem_assigned)/1024/1024/1024 mem_free_gb, (disk_assigned-disk_total)/1024/1024/1024 disk_free from __all_virtual_server_stat;
+----------+-----------------+------------------+
| cpu_free | mem_free_gb     | disk_free        |
+----------+-----------------+------------------+
| 5.5      | 8.500000000000  | -10.000000000000 |
| 5.5      | 8.500000000000  | -10.000000000000 |
| 9.5      | 13.500000000000 | 0E-12            |
+----------+-----------------+------------------+
```
